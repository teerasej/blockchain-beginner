# แสดง Article บน frontend

1. เปิด Ganache 
2. เปิด Terminal ในโปรเจค GreetingTruffle 
3. รันคำสั่ง `truffle migrate --compile-all --reset --network ganache` ใน terminal
4. เปิดไฟล์ `src\js\app.js`
5. แก้ไข `init: function()` ให้กลับเป็นเหมือนเดิม

```js
init: function() {
    return App.initWeb3();
},
```

6. เขียนโค้ดใน `initWeb3: function()`

```js
initWeb3: async function () {

          if(typeof web3 !== undefined){
               App.web3Provider = web3.currentProvider;
          } else {
               App.web3Provider = new Web3.providers.HttpProvider('http://localhost:7545');
          }
          web3 = new Web3(App.web3Provider);
          App.displayAccountInfo();
          return await App.initContract();
},
```

7. ถัดลงมา สร้าง function `displayAccountInfo()`

```js
displayAccountInfo: function() {

},
```

8. ขึ้นมาด้านบน และเพิ่ม property ชื่อ account 

```js
App = {
	web3Provider: null,
	contracts: {},
	account: 0x0,
...
}
```

9. เขียนโค้ดใช้ web3 ดึงค่าออกมาจากระบบ ในที่นี้คือ หมายเลข Account และจำนวน Ether 

```js
displayAccountInfo: function() {

        web3.eth.getCoinbase(function(err, balance){
              if(err === null){
                  console.log("No error");

                  App.account = balance;
                  console.log(balance);
                  $('#account').text(balance);

                  web3.eth.getBalance(balance, function(err, balance){
                        if(err === null){
                              console.log(balance, " Wei");
                              var ether = web3.fromWei(balance, "ether");
                              $("#accountBalance").text(ether + " Eth");
                        } else {
                              console.log(err);
                        }
                  })

              } else {
                  console.log(err);
              }
        })

     },
```

10. เขียน function `initContract()`

```js
initContract: function() {
        $.getJSON('ChainList.json', function(chainListArtifact) {
          // get the contract artifact file and use it to instantiate a truffle contract abstraction
          App.contracts.ChainList = TruffleContract(chainListArtifact);
          // set the provider for our contracts
          App.contracts.ChainList.setProvider(App.web3Provider);
          // retrieve the article from the contract
          return App.reloadArticles();
        });
      },
```

11. เขียน function `reloadArticles()`

```js
reloadArticles: function () {
          // refresh account information because the balance might have changed
          App.displayAccountInfo();

          // retrieve the article placeholder and clear it
          $('#articlesRow').empty();

          App.contracts.ChainList.deployed().then(function (instance) {
               return instance.getArticle();
          }).then(function (article) {
               if (article[0] == 0x0) {
                    // no article
                    return;
               }

               // retrieve the article template and fill it
               var articleTemplate = $('#articleTemplate');
               articleTemplate.find('.panel-title').text(article[1]);
               articleTemplate.find('.article-description').text(article[2]);
               articleTemplate.find('.article-price').text(web3.fromWei(article[3], "ether"));

               var seller = article[0];
               if (seller == App.account) {
                    seller = "You";
               }
               articleTemplate.find('.article-seller').text(seller);

               // add this article
               $('#articlesRow').append(articleTemplate.html());
          }).catch(function (err) {
               console.error(err.message);
          });
     },

```

12. ทดลองปรับ Smart Contract `ChainList.sol` เพื่อสร้างข้อมูลจำลอง

```js
contract ChainList {
...
	constructor() public {
        sellArticle("Sample Item", "This is an Item for sale", 10000000000000000000);
    }
...
}
```

13. Deploy Migration ใหม่

```pwsh
truffle migrate --compile-all --reset --network ganache
```

14.  ลอง Reload หน้าเว็บ น่าจะเห็น Article จำลองที่ได้จาก Constructor แสดงขึ้นมาบนหน้าเว็บแล้ว
