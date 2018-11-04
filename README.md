# blockchain-beginner
Example code workshop for Blockchain for developer training at https://www.nextflow.in.th

## Day 1 Workshop

1. เปิดโฟลเดอร์ workshop **greetingTruffle** ใน Terminal
2. สังเกตดูโครงสร้างของโปรเจค
3. รันคำสั่ง deploy Smart Contract ตัวอย่าง

```js
truffle migrate --network ganache
```

4. สังเกตดูค่า account 1 ใน Ganache 
5. สังเกตโฟลเดอร์ build ในโปรเจค
6. กลับมาที่ terminal รันคำสั่งเปิด Console 

```js
truffle console --network ganache
```

7. รันคำสั่งดู Address ของ Smart Contract 

```js
Greetings.address
```

8. รันคำสั่งดึง Smart Contract มาเก็บไว้ในตัวแปร `app`

```js
Greetings.deployed().then(function(instance){ app = instance;})
```

9. รันคำสั่งเรียกดู Smart Contract ในตัวแปร `app`

```js
app
```

10. รันคำสั่งเรียกใช้ Function ใน Smart Contract `getGreeting()`

```js
app.getGreeting()
```

11. รันคำสั่งเรียกใช้ Function ใน Smart Contract `setGreeting(msg)`

```js
app.setGreeting("Hello Nextflow", {from: web3.eth.accounts[0]})
```


12. กลับมาที่ Ganache และสังเกตความเปลี่ยนแปลง
13. รันคำสั่งเรียกใช้ Function ใน Smart Contract `getGreeting()` อีกครั้ง

```js
app.getGreeting()
```

14. ทดลองรันคำสั่งเรียกใช้ Function ใน Smart Contract `setGreeting(msg)` โดยเปลี่ยนหมายเลข Account และสังเกตการเปลี่ยนแปลง

```js
app.setGreeting("Jack Sparrow", {from: web3.eth.accounts[1]})

app.setGreeting("I'm number one", {from: web3.eth.accounts[2]})
```

