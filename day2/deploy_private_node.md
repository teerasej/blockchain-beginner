# ตั้ง Node บน Windows

## 1. สร้าง Genesis Block 

1. สร้าง folder ผ่าน Powershell

```bash
mkdir -p training/private
cd training/private
```

2. รันคำสั่ง

```bash
puppeth
```

3. ตั้งชื่อ Network

ไม่อนุญาตให้ใช้ตัวพิมพ์ใหญ่, เครื่องหมาย `-`, หรือช่องหว่าง (space)

```bash
nextflow_network
```

4. กด 2 เพื่อเลือกคำสั่งสร้าง Configure new genesis
5. กด 1 เลือก `Ethash - proof of work`

6. ปล่อยค่า accounts pretended เป็น `0x` (หมายถึง Account แรกในระบบ, index 0)
7. ใส่มูลค่าเข้าไปในบัญชีเริ่มต้นของระบบ พิมพ์ `yes` และกด enter
8. กำหนด network ID เป็น `4224`


## 2. Export genesis block

1. เลือก `2. Manage existing genesis`
2. เลือก `2. Export genesis configuration`
3. กำหนดไฟล์ หรือปล่อยเป็นค่าเริ่มต้นก็ได้ 
4. จะเห็นข้อความสร้างไฟล์ประมาณนี้

```bash
INFO [07-02|21:45:28.143] Saved native genesis chain spec          path=nextflow_network.json
INFO [07-02|21:45:28.147] Saved genesis chain spec                 client=aleth path=nextflow_network-aleth.json
INFO [07-02|21:45:28.150] Saved genesis chain spec                 client=parity path=nextflow_network-parity.json
INFO [07-02|21:45:28.153] Saved genesis chain spec                 client=harmony path=nextflow_network-harmony.json
```

5. เรียบร้อยแล้วให้ปิด puppeth โดยการใช้ Ctrl + C

## 3. สร้าง Private Node

รันคำสั่งสร้างโปรเจคที่จัดการข้อมูลใน Private Node

```bash
geth --datadir . init nextflow_network.json
```

เราจะเห็น directory ถูกสร้างขึ้นมา นั่นคือ

- folder **geth** เก็บ network
- folder **keystore** เก็บ account

## 4. สร้าง account

รันคำสั่งสร้าง Account ใน Node

```bash
geth account new
```

หรือในกรณีที่ต้องระบุที่อยู่ของ directory เก็บ data ของ Node

```bash
geth --datadir . account new
```

กำหนด password (passPhase) (จะไม่แสดงขึ้นบนหน้าจอ)

พอสร้างเสร็จ จะได้ address ของแต่ละ account มา

```bash
Address: {a3e095d7d89990c238f47e394f4f1bdc4c8fcdff}
```

> ในที่นี้ ให้สร้างทั้งหมด 3 account

ลองใช้คำสั่งเข้าไปดูไฟล์ใน keystore

```bash
ls 
```

จะเห็นไฟล์​ account อยู่ ให้เรารันคำสั่งด้านล่างเพื่อดูรายการ Account

```bash
geth --datadir . account list
```

## 5. สั่งเปิด private node

1. [Download ไฟล์คำสั่ง](https://www.dropbox.com/s/gafk3ylkmywi9uh/startnode.cmd.zip?dl=0)
2. แตกไฟล์คำสั่งไปไว้ในโฟลเดอร​์ `private`
3. เปิดดูไฟล์ `startnode.cmd` จะเห็นการกำหนดค่าต่างๆ รวมถึงไฟล์ password ที่ชื่อ `password.sec`
4. สร้างไฟล์ `password.sec` ไว้ใน directory `private` และกำหนดรหัสที่ใช้ตอนสร้าง Genesis Block ลงไป
3. รันคำสั่ง เพื่อเริ่มการทำงานของ Node

```pwsh
.\startnode.cmd
```

## 6. ติดต่อกับ Private Node ผ่าน JavaScript Console

เปิดหน้าต่างโปรแกรม Powershell ใหม่ 

```pwsh
geth attach ipc:\\.\pipe\geth.ipc
```

รันคำสั่งดูข้อมูลใน Private Node

```js
eth.accounts
eth.coinbase
```

ดูจำนวน ether ใน account

```js
eth.getBalance(eth.coinbase)
eth.getBalance(eth.accounts[1])
```

แปลงค่า Wei เป็น Ether

```js
account0 = eth.accounts[0]
balance0 = eth.getBalance(account0)
web3.fromWei(balance0, "ether");

account1 = eth.accounts[1]
balance1 = eth.getBalance(account1)
web3.fromWei(balance1, "ether");

account2 = eth.accounts[2]
balance2 = eth.getBalance(account2)
web3.fromWei(balance2, "ether");
```

ลองโอนเงินจากบัญชี account0 (เรียกเล่นๆ ว่า coinbase) ไปยัง account1

```js
eth.sendTransaction({ from: account0, to:account1, value: web3.toWei(3,"ether") })
```

ถ้าเราพยายามโอนเงินจากบัญชีอื่น... 

```js
eth.sendTransaction({ from: account1, to:account2, value: web3.toWei(1,"ether") })
```

จะเจอ error แบบนี้

```bash
Error: authentication needed: password or unlock
```

ต้อง unlock ด้วย password ของ account นั้นก่อน

```js
personal.unlockAccount(account1, "123456")
```