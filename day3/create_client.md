# สร้าง FrontEnd 

1. เปิด terminal มาที่โฟลเดอร์ `training/chainlist`
2. รันคำสั่ง 

```js
npm install
```

3. รันคำสั่ง เพื่อรัน web server

```js
npm run dev
```

4. เปิดไฟล์ `package.json`
5. แก้ไขชื่อโปรเจค, เวอร์ชั่น, และช่ื่อเจ้าของเป็นของเรา

## สร้าง User Interface

1. เปิดไฟล์ `src\index.html`
2. คัดลอกโค้ดจาก `resources\chainlist\01\index.html` มาวาง
3. บันทึกไฟล์ และรอให้ server reload
4. สังเกต HTML ดังต่อไปนี้

- `#articlesRow`
- `#articleTemplate`
	- `.panel-title`
	- `.article-description`
	- `.article-price`
	- `.article-seller`

## จำลองข้อมูล

1. เปิดไฟล์ `src\js\app.js`
2. เขียนโค้ดลงไปใน `init: function(){}`

```js
init: function() {
          // load articleRow
          var articlesRow = $('#articlesRow');
          var articleTemplate = $('#articleTemplate');

          articleTemplate.find('.panel-title').text('article 1');
          articleTemplate.find('.article-description').text('Description for article 1');
          articleTemplate.find('.article-price').text('10.23');
          articleTemplate.find('.article-seller').text('0x123456');

          articlesRow.append(articleTemplate.html());

          return App.initWeb3();
     },
```

## ปรับแต่ง CSS

1. สร้างไฟล์ `src\css\app.css`
2. เขียน CSS Rule 

```css
body {
    padding-top: 40px;
}

.welcome {
    font-size: 18px;
    display: inline;
    padding: 5px;
}
```
