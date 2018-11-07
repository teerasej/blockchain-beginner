# Deploy in Rinkeby

## ทดสอบการทำงานของ Network

1. เปิด Metamask และดูรายการของ Network 
2. เปิด Terminal
3. เชื่อมต่อ Rinkeby Node

```pwsh
geth --rinkeby
```

4. สังเกตการทำงานของ node
5. ปิดการทำงาน

## สร้าง account บน Rinkeby ด้วย MetaMask


## รับ ETH ผ่าน Rinkerby Faucet

1. สร้าง Address บน Rinkerby Network ผ่าน Meta Mask
2. สมัคร Twitter และวาง address ของ account ที่สร้างลงไปใน tweet
3. Copy ลิ้งค์ของ Tweet เราจะเอาไปขอ Ether
4. เปิดเว็บ [https://www.rinkeby.io/#faucet](https://www.rinkeby.io/#faucet)
5. วาง link ของ Tweet ลงไป และกดขอ Ether จากตัวเลือกด้านขวา


## สร้าง account บน Rinkeby ผ่าน Console 

1. สร้าง account 

```pwsh
geth --rinkeby account new
```

ไฟล์ account ของ windows จะอยู่ใน `%APPDATA%\Ethereum\rinkeby`


2. รันคำสั่งสร้าง node เชื่อมกับ rinkeby network ([ดาวน์โหลดคำสั่งได้ที่นี่](https://www.dropbox.com/s/e2nme9ihlu1fbh5/startrinkeby.cmd.zip?dl=0))

```pwsh
./startrinkeby.cmd
```



## สร้าง node ที่ใช้ Deploy Smart Contract

1. รันคำสั่งสร้าง node เชื่อมกับ rinkeby network

```pwsh
./startrinkeby.cmd
```

2. เชื่อมต่อ console กับ Node

```pwsh
geth attach ipc:\\.\pipe\geth.ipc
```

3. เช็คสถานะการ sync block

```pwsh
eth.syncing
```

## Deploy Smart Contract บน Rinkeby

1. เปิดไฟล์ `truffle.js` และเพิ่ม 

```js
 rinkeby: {
            host: "localhost",
            port: 8545,
            network_id: "4", //rinkeby test network
            gas: 4700000
          },
```

2. รันคำสั่ง deploy Smart Contract

```pwsh
truffle migrate --compile-all --reset --network rinkeby
```
