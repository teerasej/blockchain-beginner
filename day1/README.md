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

1. เปิด Terminal (Command Line) 
2. ถ้ายังไม่ได้ติดตั้ง `truffle` ให้รันคำสั่ง

```pwsh
npm i -g truffle
npm i -g ganache-cli
```

บน MacOS อาจจะต้องใช้คำสั่ง `sudo`

```bash
sudo npm i -g truffle
sudo npm i -g ganache-cli
```

3. รันคำสั่งเช็คเวอร์ชั่นของ truffle

```pwsh
truffle --version
```

### 2. เริ่มการทำงานของ Ganache

1. เปิดการทำงานของโปรแกรม Ganache
2. เช็คการทำงานของ Port 


### 3. สร้างโปรเจค Smart Contract

1. สร้างโฟลเดอร์ **my-smart-contract**
2. เปิดโฟลเดอร์ **my-smart-contract** ใน Terminal
3. สังเกตดูโครงสร้างของโปรเจค
4. เปิดไฟล์ **truffle-config.js**
5. กำหนดข้อมูลส่วน network ตามรายละเอียดด้านล่าง

```js
module.exports = {
    //...
    development: {
     host: "127.0.0.1",     // Localhost (default: none)
     port: 7545,            // Standard Ethereum port (default: none)
     network_id: "*",       // Any network (default: none)
    },
    //...
}
```

6. รันคำสั่ง deploy Smart Contract ตามตัวอย่าง


```bash
truffle deploy --reset
```

4. สังเกตดูค่า account 1 ใน Ganache 
5. สังเกตโฟลเดอร์ build ในโปรเจค





