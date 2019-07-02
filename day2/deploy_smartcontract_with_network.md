#  Deploy Smart Contract เข้า Ganash Network

1. ปิด truffle develop ทั้งหมด (รวมถึง --log ด้วย)
2. เปิด Ganache
3. เปิดไฟล์ `truffle-config.js` และเขียนรายละเอียด ganache ลงไป และบันทึกไฟล์

```js
module.exports = {
  // See <http://truffleframework.com/docs/advanced/configuration>
  // to customize your Truffle configuration!
  networks: {
    ganache: {
      host: "localhost",
      port: 7545,
      network_id: "*"
    }
  }
};
```

รันคำสั่งด้านล่าง เพื่อ deploy contract ลง Ganache network

```pwsh
truffle migrate --compile-all --reset --network ganache
```

เสร็จแล้วเปิดดูหน้าต่างของ Ganache จะเห็นว่า account ไม่ได้มี 100 ETH ทุกอันแล้ว

จากนั้นคลิกดู tab transaction จะเห็นว่ามีการสร้าง Contract ขึ้นมา 2 อัน

## เปิด console ของ ganache network

รันคำสั่ง

```pwsh
truffle console --network ganache
```

เช็ค Address ของ Greetings Contract

```js
Greetings.address
```

จากนั้นลองดึง Contract มาเก็บไว้ในตัวแปร App

```js
Greetings.deployed().then(function(instance){ app = instance;})
```

ทดสอบเปลี่ยน contract state ด้วยคำสั่ง

```js
account1 = (await web3.eth.getAccounts())[0]
app.setGreeting("Hello Nextflow", {from: account1})
```

และทดสอบค่าใน Contract

```js
app.getGreeting()
```

จากนั้นพิมพ์คำสั่งออกจาก console

```js
.exit
```

จากนั้นให้กลับมาที่โปรแกรม. Ganache อีกที เพื่อเช็ค transaction และจำนวน ETH ของ account 1 