# เพิ่มส่วนสร้าง Article ใหม่บนหน้าเว็บ

1. เปิดไฟล์ `app.js`
2. สร้าง function `sellArticle()`

```js
sellArticle: function() {
   
},
```

 3. เปิดไฟล์ `index.html`
4. สังเกต HTML element ต่อไปนี้
- `#article_name`
- `#article_description`
- `#article_price`

5. กลับมาที่ function `sellArticle()` และเขียนคำสั่งดึงค่าออกจาก Web form เพื่อใส่ลงไปใน Smart Contract 

```js
sellArticle: function() {
    // ถึงข้อมูลสินค้าที่จะประกาศขายผ่าน Web form 
    var _article_name = $('#article_name').val();
    var _description = $('#article_description').val();
    var _price = web3.toWei(parseFloat($('#article_price').val() || 0), "ether");

    if((_article_name.trim() == '') || (_price == 0)) {
      // ถ้าไม่มีการใส่ชื่อสินค้า หรือกำหนดราคา ก็ถือว่าไม่มีการสร้างสินค้าเพื่อขาย
      return false;
    }

    App.contracts.ChainList.deployed().then(function(instance) {
      return instance.sellArticle(_article_name, _description, _price, {
        from: App.account,
        gas: 500000
      });
    }).then(function(result) {
      App.reloadArticles();
    }).catch(function(err) {
      console.error(err);
    });
  },
```

6. กลับมาที่ไฟล์ `index.html` และมองค่าปุ่ม submit ที่อยู่ในส่วน

```html
<div class="modal-footer">
	<button ...>Submit</button>
...
</div>
```

7. เรียกใช้ function `sellArticle()`

```html
<button ... onclick="App.sellArticle(); return false;">Submit</button>
```

8. ปิด server 
9. Deploy Migration ใหม่

```pwsh
truffle migrate --compile-all --reset --network ganache
```

10. รัน server ใหม่ด้วยคำสั่ง

```pwsh
npm run dev
```

