# ค่า Gas

- จ่ายค่าโอน ค่า deploy ค่ารันธุรกรรมต่างๆ 
- คำนวนจากค่า Difficulty ในหน่วย Gas
- จ่ายโดยเจ้าของ Account ผู้เริ่มทำธุรกรรม
- 1 eth ไม่เท่ากับ 1 Gas ค่าจะถูกคำนวนเป็นค่า Wei
- ค่า Gas จะแตกต่างกันในธุรกรรมแต่ละแบบ แต่ยิ่งซับซ้อนมาก ก็ต้องการ Gas สูง
- Gas Limit จะเป็นตัวช่วยไม่ให้ Smart contract ที่เขียนไม่ดีหรือติดลูป ไม่ใช้ Gas เกินความจำเป็น
- More Gas, more performance 
- Gas จ่ายแล้ว ถ้า Transaction error ก็จะถือว่าจ่ายไปเลย เรียกคืนไม่ได้

## ทดสอบค่า Gas

1. เปิด Ganache 
2. เปิด Terminal ในโปรเจค GreetingTruffle 
3. รันคำสั่ง `truffle migrate --compile-all --reset --network ganache` ใน terminal
4. หลัง Deploy Smart contract แล้ว รันคำสั่ง `truffle console --network ganache`
5. รันคำสั่งเพื่อดูค่า Ether ใน Account 

```js
account1 = (await web3.eth.getAccounts())[0]
web3.fromWei(web3.eth.getBalance(account1)).toNumber()

account2 = (await web3.eth.getAccounts())[1]
web3.fromWei(web3.eth.getBalance(account2)).toNumber()
```

6. ดึง Smart Contract มาใช้งาน

```js
Greetings.deployed().then(function(instance) {app = instance})
```

7. ทดสอบ `getGreeting()` โดยใช้ account ที่ 2

```js
app.getGreetings({from: account2})
```

8. เช็ค eth ในแอคเค้าที่ 2 จะเห็นว่าไม่มีการใช้ eth 

เพราะการเรียกใช้ method `view` หรือการดูค่าจาก Smart Contract อย่างเดียวจะไม่เสียค่า gas (สามารถเช็คจาก Ganache ว่าจะไม่มี Transaction เกิดขึ้น)

```js

web3.fromWei(web3.eth.getBalance(account2)).toNumber()
```

9. ทดสอบเปลี่ยน state ของ Smart Contract โดยใช้คำสั่งด้านล่าง

สังเกตว่ามีค่า gas แสดงขึ้นมา โดยให้เช็คใน console และ ganache ว่า account ที่ 2 มีการเปลี่ยนแปลง และมี Transaction ปรากฎขึ้นมาในแท็บ Transaction

```js
app.setGreeting("I pay something", { from: account2 } );
```


## แปลงค่า Gas เป็นค่า Eth

[https://converter.murkin.me/](https://converter.murkin.me/)