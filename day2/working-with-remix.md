
# Working with Remix

เปิดเว็บ 

## 1. สร้างไฟล์ใหม่ใน contracts directory

ตั้งชื่อว่า `helloWorld.sol`

```solidity
pragma solidity >=0.4.22 <0.9.0;

contract HelloWorld {
    string private helloMessage = "Hello World";

    function getHelloMessage() public view returns (string memory) {
        return helloMessage;
    }
}
```

## 2. การใช้งานส่วน compiler

1. จากเมนูด้านข้างให้
2. เราสามารถเลือกเวอร์ชั่นของ compiler ได้
3. ถ้าต้องการสามารถกดเลือก Auto compile ได้
4. ถ้าต้องการกด compile สามารถเลือกไฟล์ และกดปุ่ม compile ได้

<img width="371" alt="2022-05-09_17-19-05" src="https://user-images.githubusercontent.com/85179/167390707-bf62b1d7-bf67-489e-94c1-78b474a39d44.png">

การ compile เราจะได้สิ่งที่เรียกว่า artifacts ซึ่งเป็นไฟล์ JSON ที่สามารถอ้างอิงถึงตัว Smart Contract ได้

## 3. การใช้งานส่วน Deploy & Run transaction

1. เปิดส่วนของ Deploy & Run transaction
2. Environment 
   1. เลือก Ganache Provider
   2. กำหนดต่อเข้ากับ Endpoint: **http://127.0.0.1:7545**
3. สามารถเลือก Account ใน Network ได้
4. เลือก contract ที่ต้องการ และกด deploy
5. Contract ที่ deploy แล้วจะแสดงขึ้นมาด้านล่าง สามารถกดรัน function หรือ variable ได้
6. แสดงค่าที่คืนออกมาจาก contract

<img width="370" alt="2022-05-09_17-23-14" src="https://user-images.githubusercontent.com/85179/167391540-903772bb-e8f4-47d7-ad46-3aae1dae9820.png">

