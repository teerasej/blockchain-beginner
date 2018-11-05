# blockchain-beginner
Example code workshop for Blockchain for developer training at https://www.nextflow.in.th

## Resource

- [Presentation](https://www.dropbox.com/s/kzoa891iv5dedif/Blockchain%20for%20Developer.pdf?dl=0)

## Day 1 Workshop

- [Project](https://www.dropbox.com/s/scnhm7qse3uxl6v/greetingTruffle.zip?dl=0)
- [Bitcoin stolen tracking](https://www.reddit.com/r/Bitcoin/comments/5vr8xg/confirmed_120000_bitcoins_from_the_bitfinex_hack/de4e4nf/)
- [CryptoKitties](https://www.cryptokitties.co/)

### Check truffle

1. เปิด Terminal (Powershell) 
2. รันคำสั่งเช็คเวอร์ชั่นของ trufflee

```pwsh
truffle --version
```

3. ถ้ายังไม่ได้ติดตั้ง ให้รันคำสั่ง

```pwsh
npm i -g truffle
```

### Let's try Smart Contract

1. เปิดโฟลเดอร์ workshop **greetingTruffle** ใน Terminal
2. สังเกตดูโครงสร้างของโปรเจค
3. รันคำสั่ง deploy Smart Contract ตัวอย่าง


```pwsh
truffle migrate --network ganache
```

4. สังเกตดูค่า account 1 ใน Ganache 
5. สังเกตโฟลเดอร์ build ในโปรเจค
6. กลับมาที่ terminal รันคำสั่งเปิด Console 

```pwsh
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

