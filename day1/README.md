# World of Blockchain

## Resource

- [Bitcoin stolen tracking](https://www.reddit.com/r/Bitcoin/comments/5vr8xg/confirmed_120000_bitcoins_from_the_bitfinex_hack/de4e4nf/)
- [CryptoKitties](https://www.cryptokitties.co/)

## Setup MetaMask

- [วิธีการติดตั้ง MetaMask](https://github.com/teerasej/blockchain-beginner/blob/master/day1/install_metamask.md)

## Project Files

- [Project](https://www.dropbox.com/s/txx1s9s588gr1ex/greetingTruffle2.zip?dl=0)


## Workshop

### 1. Check truffle version

1. เปิด Terminal (Powershell) 
2. รันคำสั่งเช็คเวอร์ชั่นของ truffle

```pwsh
truffle --version
```

3. ถ้ายังไม่ได้ติดตั้ง `truffle` ให้รันคำสั่ง

```pwsh
npm i -g truffle
npm i -g ganache-cli
```

### 2. Let's deploy Smart Contract

1. เปิดโฟลเดอร์ workshop **greetingTruffle** ใน Terminal
2. สังเกตดูโครงสร้างของโปรเจค
3. รันคำสั่ง deploy Smart Contract ตามตัวอย่าง


```pwsh
truffle migrate --network ganache
```

4. สังเกตดูค่า account 1 ใน Ganache 
5. สังเกตโฟลเดอร์ build ในโปรเจค
6. กลับมาที่ terminal รันคำสั่งเปิด Console 

```pwsh
truffle console --network ganache
```

### 3. ดูรายละเอียดของ Smart Contract ใน Network

1. รันคำสั่งดู Address ของ Smart Contract 

```js
Greetings.address
```

2. รันคำสั่งดึง Smart Contract มาเก็บไว้ในตัวแปร `app`

```js
Greetings.deployed().then(function(instance){ app = instance;})
```

3. รันคำสั่งเรียกดู Smart Contract ในตัวแปร `app`

```js
app
```

### 4. ใช้งาน Function ที่เขียนไว้ใน Smart Contract

1. รันคำสั่งเรียกใช้ Function ใน Smart Contract `getGreeting()`

```js
app.getGreeting()
```

2. รันคำสั่งเรียกใช้ Function ใน Smart Contract `setGreeting(msg)`

```js
account0 = (await web3.eth.getAccounts())[0]
app.setGreeting("Hello Nextflow", {from: account0})
```

3. กลับมาที่ Ganache และสังเกตความเปลี่ยนแปลง
4. รันคำสั่งเรียกใช้ Function ใน Smart Contract `getGreeting()` อีกครั้ง

```js
app.getGreeting()
```

5. ทดลองรันคำสั่งเรียกใช้ Function ใน Smart Contract `setGreeting(msg)` โดยเปลี่ยนหมายเลข Account และสังเกตการเปลี่ยนแปลง

```js
account1 = (await web3.eth.getAccounts())[1]

app.setGreeting("Tony Stark", {from: account1})

account2 = (await web3.eth.getAccounts())[2]
app.setGreeting("I'm number one", {from: account1})
```


