# ทำงานกับ Event 

## 1. สร้าง Event ใน Smart Contract

1. เปิดไฟล์ `contracts\ChainList.sol`
2. เพิ่ม event function 

```js
// events
  event LogSellArticle(
    address indexed _seller,
    string _name,
    uint256 _price
  );
  event LogBuyArticle(
    address indexed _seller,
    address indexed _buyer,
    string _name,
    uint256 _price
  );
```
3. ใน function `sellArticle()` ให้เรียกใช้ event function `LogSellArticle()` เพื่อแจ้งเตือนระบบ

```js
function sellArticle(string _name, string _description, uint256 _price) public {
    seller = msg.sender;
    name = _name;
    description = _description;
    price = _price;

    LogSellArticle(seller, name, price);
}
```


5. ถัดลงมาเพิ่ม function `buyArticle()`

```js
// buy an article
  function buyArticle() payable public {
    // เช็คว่า มีผู้ขาย
    require(seller != 0x0);

    // เช็คว่าสินค้ายังไม่ถูกซืื้อ
    require(buyer == 0X0);

    // กำหนดให้ผู้ขาย ไม่สามารถซื้อสินค้าของตัวเองได้
    require(msg.sender != seller);

    // เช็คว่าค่าที่ส่งมากับ transaction เท่ากับราคาของสินค้า
    require(msg.value == price);

    // เก็บข้อมูลของผู้ซื้อ
    buyer = msg.sender;

    // อณุญาตให้ผู้ซื้อโอนจำนวนให้ผู้ขายได้
    seller.transfer(msg.value);

    // สร้าง Event แจ้งเตือน
    LogBuyArticle(seller, buyer, name, price);
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

```js
reloadArticles: function () {
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
               articleTemplate.find('.article-price').text(web3.fromWei(article[4], "ether"));

               var seller = article[0];
               if (seller == App.account) {
                    seller = "You";
               }
               articleTemplate.find('.article-seller').text(seller);

                // เพิ่มส่วนของ buyer
                var buyer = article[1];
                if(buyer == App.account){
                    buyer = "You";
                } else if(buyer == 0X0) {
                    buyer = "No one yet";
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

    App.contracts.ChainList.deployed().then(function(instance){
      return instance.buyArticle({
        from: App.account,
        value: web3.toWei(_price, "ether"),
        gas: 500000
      });
    }).catch(function(error) {
      console.error(error);
    });
  }
```