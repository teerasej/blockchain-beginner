
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