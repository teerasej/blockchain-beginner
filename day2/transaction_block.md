# ดูรายละเอียดของ Transaction และ Block

1. เปิด Ganache 
2. เปิด Terminal ในโปรเจค GreetingTruffle 
3. รันคำสั่ง ใน terminal

```pwsh
truffle migrate --compile-all --reset --network ganache 
```

4. หลัง Deploy Smart contract แล้ว รันคำสั่ง

```pwsh
truffle console --network ganache
```

5. กลับมาที่ Ganache คลิกแท็บ Transaction และคลิกดูรายละเอียดต่างๆ ด้านใน Transaction
6. คัดลอกหมายเลข Transaction Hash 
7. กลับมาที่ Console 
8. รันคำสั่ง `web3.eth.getTransaction("<Tx hash>")` โดยวาง Transaction Hash ที่คัดลอกมาลงไปในส่วน `<Tx hash>`
9. กลับมาที่ Ganache เปิดแท็บ Block
10. คลิกดูข้อมูลของ Block 
11. คัดลอก Block hash มาใช้งาน
12. รันคำสั่ง `web3.eth.getBlock("<block hash>")` โดยวาง Block Hash ที่คัดลอกมาลงไปในส่วน `<block hash>`
13. ดูข้อมูลของ Block