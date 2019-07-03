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
     if (window.ethereum) {
          App.web3Provider = window.ethereum;
          try {
               // Request account access
               await window.ethereum.enable();
          } catch (error) {
               // User denied account access...
               console.error("User denied account access")
          }
     }
     // Legacy dapp browsers...
     else if (window.web3) {
          App.web3Provider = window.web3.currentProvider;
     }
     else {
          console.log('MetaMask not ready, use ganache');
          App.web3Provider = new Web3.providers.HttpProvider('http://localhost:7545');
     }
     console.log('Web3 provider:', App.web3Provider)
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
displayAccountInfo: async function () {

     web3.eth.getCoinbase(function (err, balance) {
          if (err === null) {
               console.log("No error", balance);

               App.account = balance;
               console.log(balance);
               $('#account').text(balance);

               web3.eth.getBalance(balance, function (err, balance) {
                    if (err === null) {
                         // console.log(balance, " Wei");
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
     // สร้าง object ตัวแทนของ Smart Contract 
     App.contracts.ChainList = TruffleContract(chainListArtifact);
     // กำหนดแหล่งบัญชีผู้ใช้ที่จะทำงานกับ Contract
     App.contracts.ChainList.setProvider(App.web3Provider);
     // เรียกดูสินค้า
     return App.reloadArticles();
     });
},
```

11. เขียน function `reloadArticles()`

```js
reloadArticles: function () {
     
          // ป้องกันการโหลดข้อมูลซ้ำ
          if(App.loading) {
               return;
          }
          App.loading = true;

          // เรียกดูข้อมูลผู้ใช้
          App.displayAccountInfo();

          // ล้างข้อมูลสินค้าบนหน้าเว็บ
          $('#articlesRow').empty();

          App.contracts.ChainList.deployed().then(function (instance) {
               return instance.getArticle();
          }).then(function (article) {
               if (article[0] == 0x0) {
                    // ไม่มีข้อมูลสินค้า ไม่แสดง
                    return;
               }

               console.log(article);

               // ดึงข้อมูลสินค้ามาแสดงบนหน้าเว็บ
               var articleTemplate = $('#articleTemplate');
               articleTemplate.find('.panel-title').text(article[2]);
               articleTemplate.find('.article-description').text(article[3]);
               var price = web3.fromWei(article[4], "ether");
               articleTemplate.find('.article-price').text(price);

               var seller = article[0];
               if (seller == App.account) {
                    seller = "You";
               }
               articleTemplate.find('.article-seller').text(seller);

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
