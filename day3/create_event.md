# ทำงานกับ Event ใน Smart Contract

## 1. สร้าง Event ใน Smart Contract

1. เปิดไฟล์ `contracts\ChainList.sol`
2. เพิ่ม event function 

```js
// events
  event LogSellArticle(
    address indexed _seller,
    string _name,
    uint _price
  );
  event LogBuyArticle(
    address indexed _seller,
    address indexed _buyer,
    string _name,
    uint _price
  );
```
3. ใน function `sellArticle()` ให้เรียกใช้ event function `LogSellArticle()` เพื่อแจ้งเตือนระบบ

```js
function sellArticle(string memory _name, string memory _description, uint _price) public {
    seller = msg.sender;
    name = _name;
    description = _description;
    price = _price;

    LogSellArticle(seller, name, _price);
}
```


5. ถัดลงมาเพิ่ม function `buyArticle()`

```js
// buy an article
  function buyArticle() public payable  {
    
    require(seller != 0x0, "Seller should not be empty");

    //require(buyer == 0X0, "Buyer should not be empty");

    require(msg.sender != seller, "Buyer's address should not be seller");
    require(msg.value == price, "given value should equal to item's price");

    buyer = msg.sender;
    seller.transfer(msg.value);

    emit LogBuyArticle(seller, buyer, name, price);
  }
```

## 2. ปรับแต่งส่วนของ Web Front-end

1. เปิดไฟล์ `src\js\app.js`
2. ใน function `initContract()` ให้เพิ่มการเรียกใช้คำสั่ง `App.listenToEvents();`

```js
initContract: function() {
        $.getJSON('ChainList.json', function(chainListArtifact) {
          // get the contract artifact file and use it to instantiate a truffle contract abstraction
          App.contracts.ChainList = TruffleContract(chainListArtifact);
          // set the provider for our contracts
          App.contracts.ChainList.setProvider(App.web3Provider);
          
          
          // listen to events
          App.listenToEvents();
          
          
          // retrieve the article from the contract
          return App.reloadArticles();
        });
      },
```

3. แก้ไข function `reloadArticles()`

โดยการ **แทรกราคาเข้าไปในปุ่ม** และเพิ่มส่วนแสดงผลของ Buyer

```js
reloadArticles: function () {
          App.displayAccountInfo();

          $('#articlesRow').empty();

          App.contracts.ChainList.deployed().then(function (instance) {
               return instance.getArticle();
          }).then(function (article) {
               if (article[0] == 0x0) {
                    
                    return;
               }

               console.log(article);

               var articleTemplate = $('#articleTemplate');
               articleTemplate.find('.panel-title').text(article[2]);
               articleTemplate.find('.article-description').text(article[3]);
               var price = web3.fromWei(article[4], "ether");
               articleTemplate.find('.article-price').text(price); 

               // แทรกราคาเข้าไปใน ปุ่ม
               articleTemplate.find('.btn-buy').attr('data-value', price);

               var seller = article[0];
               if (seller == App.account) {
                    seller = "You";
               }
               articleTemplate.find('.article-seller').text(seller);

                // เพิ่มส่วนแสดงผลของ buyer
                var buyer = article[1];
                if(buyer == 0x0){
                    buyer = "No one yet";
                } else if(buyer == App.account) {
                    buyer = "You";
                }
                articleTemplate.find('.article-buyer').text(buyer);

                if(article[0] == App.account || article[1] != 0X0) {
                    articleTemplate.find('.btn-buy').hide();
                } else {
                    articleTemplate.find('.btn-buy').show();
                }

               $('#articlesRow').append(articleTemplate.html());
          }).catch(function (err) {
               console.error(err.message);
          });
     },

```


4. เขียน function `listenToEvents()` เพื่อเฝ้าดู event ที่สามารถแจ้งมาจากฝั่ง Smart Contract ได้

```js
// ฟัง event ที่สามารถแจ้งเตือนจากตัว contract ได้
  listenToEvents: function() {

    App.contracts.ChainList.deployed().then(function(instance) {
      
      // เฝ้าดู event จาก LogSellArticle() ของ Contract
      instance.LogSellArticle({}, {}).watch(function(error, event) {
        if (!error) {
          $("#events").append('<li class="list-group-item">' + event.args._name + ' is now for sale</li>');
        } else {
          console.error(error);
        }
        App.reloadArticles();
      });

      // เฝ้าดู event จาก LogBuyArticle() ของ Contract
      instance.LogBuyArticle({}, {}).watch(function(error, event) {
        if (!error) {
          $("#events").append('<li class="list-group-item">' + event.args._buyer + ' bought ' + event.args._name + '</li>');
        } else {
          console.error(error);
        }
        App.reloadArticles();
      });
    });
  },
```

4. เพิ่ม function `buyArticle()` 


```js
buyArticle: function() {
    event.preventDefault();

    // retrieve the article price
    var _price = parseFloat($(event.target).data('value'));
    console.log("Item's Price: ", _price);
    console.log("Buyer id:", App.account);

    App.contracts.ChainList.deployed().then(function(instance){
      return instance.buyArticle({
        from: App.account,
        value: web3.toWei(_price, "ether"),
        gas: 500000
      });
    }).then(function(){
      App.reloadArticles();
    }).catch(function(error) {
      console.error(error);
    });
  }
```