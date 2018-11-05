# ทดสอบ Smart Contract 

- **Smart Contract ที่ Deploy ไปแล้ว ไม่สามารถเปลี่ยนแปลง code ได้** นอกจากต้อง ปิด Contract และ Deploy เป็น Contract ใหม่ (ได้ address ใหม่ ไม่ใช่ของเดิม
- ปัจจุบันยังไม่มี Analysis tools สำหรับ monitor Smart Contract ใน Network
- Test จึงเป็นหนทางที่ดีที่สุด

## เขียน Test Unit แรกให้กับ Smart Contract

1. เปิดโฟลเดอร์ `chainlist/test`
2. สร้างไฟล์ `ChainListHappyPath.js`
3. เขียน Test Suite

```js
var ChainList = artifacts.require("./ChainList.sol");

//test suite
contract('ChainList', function(accounts){
	it("should be init with empty value", function() {
        return ChainList.deployed().then(function(instance){
            return instance.getArticle();
        }).then(function(data){
            assert.equal(data[0], 0x0, "Seller must be empty");
            assert.equal(data[1], "", "article name must be empty");
            assert.equal(data[2], "", "article description must be empty");
            assert.equal(data[3].toNumber(), 0, "article price must be 0");
        });
    });
});
```

- สังเกตว่า parameter แรกเป็นชื่อของ Contract ที่ต้องการแสดง 
- function test รับตัวแปร account เข้ามาเพื่อเรียกใช้ในการทดสอบได้

4. เขียน. Test unit แรกโดยใช้ `it()`

```js
...

//test suite
contract('ChainList', function(accounts){

	it("should be init with empty value", function() {
        
    });
});
```

5. เรียกใช้งาน function ของ Smart Contract (สังเกตว่าเป็น function เดียวกันกับที่ใช้ใน console) 

```js
var ChainList = artifacts.require("./ChainList.sol");

//test suite
contract('ChainList', function(accounts){
	it("should be init with empty value", function() {

        return ChainList.deployed().then(function(instance){
            return instance.getArticle();
        })

    });
});
```

7. เขียน chain function `.then()` ที่รับค่าจาก `instance.getArticle()` และใช้คำสั่ง `assert.equal()` ในการเช็คค่า

```js
...
//test suite
contract('ChainList', function(accounts){
	it("should be init with empty value", function() {

        return ChainList.deployed().then(function(instance){
            return instance.getArticle();
        }).then(function(data){
            assert.equal(data[0], 0x0, "Seller must be empty");
            assert.equal(data[1], "", "article name must be empty");
            assert.equal(data[2], "", "article description must be empty");
            assert.equal(data[3].toNumber(), 0, "article price must be 0");
        });

    });
});
```

8.  บันทึกไฟล์

## ทดสอบ Test Suite

- Truffle จะ deploy Smart Contract แยกในแต่ละ Test case และ Test Suite ทำให้ Smart Contract ที่ได้เป็นคนละตัวกัน 
- มีการจ่าย Gas ในกรณีเดียวกับการใช้ Contract จริง **จึงไม่ควรรัน test บน Network จริง (เสีย eth จริง)**

1. เปิด Ganache
2. รันคำสั่ง test 

```pwsh
truffle test --network ganache
```

3. น่าจะเห็นข้อความดังภาพ ในที่นี้

```pwsh
PS C:\Users\teerasej\ChainSkills\training\chainlist> truffle test --network ganache
Using network 'ganache'.



  Contract: ChainList
    √ should be init with empty value


  1 passing (107ms)

```

เป็นการแสดงว่า ค่าที่ได้จาก Smart Contract ผ่านการทดสอบในกรณีที่เขียนไว้ใน Test Suite

4. ให้ลองเปลี่ยนค่าใน assert ด้านล่าง จาก 0 เป็น 1 

```js
assert.equal(data[3].toNumber(), 1, "article price must be 0");
```

5. บันทึกไฟล์ และรันคำสั่ง `truffle test --network ganache` ใหม่ จะเห็นการรันเทสที่ไม่ผ่าน

## เพิ่ม Test case สำหรับ Sell Article 

1. จากไฟล์ `ChainListHappyPath.js` เพิ่มตัวแปรเพื่อใช้ในการเขียน test case

```js
//test suite
contract('ChainList', function(accounts){

    var chainListInstance;
    var seller = accounts[1];
    var articleName = "article 1";
    var articleDescription = "Description for article 1";
    var articlePrice = 10;

	...
```

- สังเกตว่าตัวแปร seller รับค่า Account ที่ 2 ใน Network Ganache เข้ามาใช้งาน

2. เพิ่ม test unit ตัวที่สองเข้าไปใน test suite


```js
it("should be init with empty value", function() {
       ...
    });

    it("should sell an article", function(){

        
    });
```

เก็บ instance ของ Smart Contract ไว้ในตัวแปร `chainListInstance` เพื่อใช้ที่อื่น และสั่งเรียกใช้ `sellArticle()` เพื่อเปลี่ยนแปลงข้อมูล

```js


    it("should sell an article", function(){

        return ChainList.deployed().then(function(instance){
            chainListInstance = instance;
            return chainListInstance.sellArticle(articleName, articleDescription, web3.toWei(articlePrice, "ether"), {from: seller});
        })

    });
```

ถัดจากการเรียกใช้ `sellArticle()` เราจะเรียกใช้ `getArticle()`

```js

it("should sell an article", function(){

        return ChainList.deployed().then(function(instance){
            chainListInstance = instance;
            return chainListInstance.sellArticle(articleName, articleDescription, web3.toWei(articlePrice, "ether"), {from: seller});
        }).then(function(data){
            return chainListInstance.getArticle();
        })
 });
```

และเขียนเช็คค่า เทียบกับข้อมูลที่กำหนด

```js

it("should sell an article", function(){

        return ChainList.deployed().then(function(instance){
            chainListInstance = instance;
            return chainListInstance.sellArticle(articleName, articleDescription, web3.toWei(articlePrice, "ether"), {from: seller});
        }).then(function(data){
            return chainListInstance.getArticle();
        }).then(function(data){
            assert.equal(data[0], seller, "Seller must be " + seller);
            assert.equal(data[1], articleName, "article name must be " + articleName);
            assert.equal(data[2], articleDescription, "article description must be " + articleDescription);
            assert.equal(data[3].toNumber(), web3.toWei(articlePrice,"ether"), "article price must be " + web3.toWei(articlePrice,"ether"));
        });
});
```

3. บันทึกไฟล์ และรันคำสั่ง `truffle test --network ganache` ใหม่ 