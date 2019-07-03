# Create Smart Contract with truffle

1. สร้างโฟลเดอร์ `training/GreetingsTruffle`
2. เปิด Powershell หรือ Terminal มาที่โฟลเดอร์ดังกล่าว
3. รันคำสั่ง check `truffle --version`
4. รันคำสั่ง `truffle init`

## structure

- **Contracts** ใช้ track การ deploy contract ใน network ต่างๆ และกันไม่ให้ contract สูญหาย
- **migrations** เก็บ logic ที่ใช้ในการ deploy smart contract
- **test** เก็บ test Script 
- `truffle-config.js` กำหนดค่า environment และ network ปกติจะโล่ง นอกจากจะมากำหนดเพิ่ม

## 1. สร้างไฟล์ Smart Contract

1. สร้างไฟล์ใหม่ในโฟลเดอร์ `Contracts`
2. ตั้งชื่อไฟล์ว่า `greetings.sol`
3. เขียนโค้ดด้านล่าง

```js
//solium-disable linebreak-style
pragma solidity ^0.5.8;

contract Greetings {
    string message;

    constructor() public {
        message = "OK!";
    }

    function setGreeting(string memory _message) public {
        message = _message;
    }

    function getGreeting() public view returns (string memory) {
        return message;
    }
}
```

## 2. สร้างไฟล์ Migration

1. สร้างไฟล์ใหม่ในโฟลเดอร์​ `migrations`
2. ตั้งชื่อว่า `2_deploy_contract.js`
2. เขียนโค้ดด้านล่าง

```js
var Greetings = artifacts.require("./greetings.sol");

module.exports = function(deployer){
    deployer.deploy(Greetings);
}
```

## 3. เปิด develop mode

1. จาก terminal โปรเจค
2. รันคำสั่ง `truffle develop --log`
3. จะเห็นว่า truffle ใช้กลไกเดียวกับ Ganash โดยการสร้าง 10 account ขึ้นมา

## 4. คำสั่ง truffle

คำสั่งที่แก้ปัญหาส่วนใหญ่ได้ โดยการสั่ง compile contract ทั้งหมด และ deploy ใหม่ตาม script 

```pwsh
truffle migrate --compile-all --reset
```

1. รันคำสั่ง `truffle develop —log` ใน terminal 
2. ทดลองสั่งคำสั่งใน `truffle develop` ในอีก terminal
3. จะสังเกตว่า มีการ compile และ deploy contract 
4. ลองกลับมาดูใน `--log` จะเห็นการทำงานภายใน network
5. กลับมาที่ truffle develop และสั่ง `migrate`
6. จะเห็นข้อความว่า network up to date 

> คำสั่ง migrate จะไม่ re-deploy หรือ compile contract ถ้ามีการ deploy ใน network นี้ก่อนหน้าแล้ว

## 5. ใช้งาน Contracts จาก Develop console

1. จาก develop console สั่ง `Greetings.address` 
2. จะเห็น address ของ contract แสดงขึ้นมา
3. ลองดูในโปรเจค จะเห็นโฟลเดอร์ `build` ในนี้จะมีไฟล์ json ที่แสดงรายละเอียดของ contract ต่างๆ 
4. ลองเปิดไฟล์ `Greetings.json` ด้านล่างสุด จะเห็นส่วนระบุข้อมูล network ในที่นี้เป็นของ truffle develop 

## 6. ทดสอบเรียกใช้งาน Contract

จาก develop console รันคำสั่ง

```js
Greetings.deployed().then(function(instance){ app = instance;})
```

ทดสอบรันคำสั่ง `app` จะเห็นรายละเอียดของ Contract รวมถึง function ที่เราประกาศไว้ใน contract ด้วย

ทดลองรันคำสั่ง `app.getGreeting()` จะเห็็นข้อความที่เรากำหนดใน constructor ของ Contract แสดงขึ้นมา

> การเรียกใช้ `getGreeting()` ไม่ได้เป็นการเปลี่ยนแปลง contract (กำหนดไว้เป็น view method) ซึ่งจะไม่มีการส่ง transaction เข้าไปใน network เป็นแค่การเช็ค state ว่าจะเหมือนกันในทุก node

## มาเปลี่ยน Contract State กัน

ทดสอบเปลี่ยน contract state ด้วยคำสั่ง

```js
account1 = (await web3.eth.getAccounts())[0]
app.setGreeting("Hello Nextflow", {from: account1})
```

จะได้ transaction message กลับมา

> สังเกตว่าจริงๆ `setGreeting()` ที่เราสร้างขึ้นมารับแค่ parameter เดียว แต่ parameter ตัวที่ 2 สร้างขึ้นโดย truffle 

> การสั่ง function ของ Contract รอบนี้จะมี Transaction เกิดขึ้น และถูกส่งเข้าไปใน network และ node ที่ทำงานอยู่

เช็ค greeting message อีกที

```js
app.getGreeting()
```

จะเห็นว่าข้อความเปลี่ยนไป

