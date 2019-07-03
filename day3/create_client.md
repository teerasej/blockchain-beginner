# สร้าง FrontEnd 

## 1. เปิดการทำงานของ Web Server

1. เปิด terminal มาที่โฟลเดอร์ `training/chainlist`
2. รันคำสั่ง 

```js
npm install
```

3. รันคำสั่ง เพื่อรัน web server

```js
npm run dev
```

4. ระบบจะเปิด Web Browser ขึ้นมาแสดงหน้าเว็บต้นแบบ

## 2. สร้าง User Interface

1. เปิดไฟล์ `src\index.html`
2. [เปิดไฟล์ index.html](https://github.com/teerasej/blockchain-beginner/blob/master/day3/src/index.html) และคัดลอกโค้ดที่เตรียมไว้มาวางในโปรเจคเพื่อสร้าง UI
3. บันทึกไฟล์ และรอให้ server reload
4. สังเกต HTML ดังต่อไปนี้

- `#articlesRow`
- `#articleTemplate`
	- `.panel-title`
	- `.article-description`
	- `.article-price`
	- `.article-seller`

## 3. จำลองข้อมูล

1. เปิดไฟล์ `src\js\app.js`
2. เขียนโค้ดลงไปใน `init: function(){}`

```js
init: function() {
          // แสดงข้อมูลจำลองบนหน้าเว็บ
          var articlesRow = $('#articlesRow');
          var articleTemplate = $('#articleTemplate');

          articleTemplate.find('.panel-title').text('article 1');
          articleTemplate.find('.article-description').text('Description for article 1');
          articleTemplate.find('.article-price').text('10.23');
          articleTemplate.find('.article-seller').text('0x123456');
          articleTemplate.find('.article-buyer').text('0x123456')

          articlesRow.append(articleTemplate.html());

          return App.initWeb3();
     },
```

## 4. ปรับแต่ง CSS

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

## 5. ทดสอบหน้าเว็บแอพ

ทำการ refresh หน้าเว็บเพื่อให้เป็นตัวอย่างของ Contract เวลาแสดงขึ้นมาบนหน้าเว็บ
