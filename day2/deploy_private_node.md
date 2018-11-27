# ตั้ง Node บน Windows

## สร้าง Genesis Block 

1. สร้าง folder ผ่าน Powershell

```bash
mkdir -p ~/trainig/private
cd training/private
```

2. รันคำสั่ง

```bash
puppeth
```

3. ตั้งชื่อ Network

```bash
ChainSkills
```

4. กด 2 เพื่อเลือกคำสั่งสร้าง Configure new genesis
5. กด 1 เลือก `Ethash - proof of work`

6. ปล่อยค่า accounts pretended เป็น `0x`

7. กำหนด network ID เป็น `4224`


## Export genesis block

1. เลือก `2. Manage existing genesis`
2. เลือก `2. Export genesis configuration`
3. กำหนดไฟล์ หรือปล่อยเป็นค่าเริ่มต้นก็ได้ 
4. จะเห็นข้อความ ` Exported existing genesis block`
5. เรียบร้อยแล้วให้ปิด puppeth โดยการใช้ Ctrl + C

## สร้าง Private Node

รันคำสั่งสร้างโปรเจคที่จัดการข้อมูลใน Private Node

```bash
geth --datadir . init Nextflow.json
```

- folder **geth** เก็บ network
- folder **keystore** เก็บ account

## สร้าง account

รันคำสั่งสร้าง Account ใน Node

```bash
geth --datadir . account new
```

กำหนด password (passPhase) (จะไม่แสดงขึ้นบนหน้าจอ)

พอสร้างเสร็จ จะได้ address ของแต่ละ account มา

```bash
Address: {a3e095d7d89990c238f47e394f4f1bdc4c8fcdff}
```

> สร้างทั้งหมด 3 account

ลองใช้คำสั่งเข้าไปดูไฟล์ใน keystore

```bash
ls 
```

จะเห็นไฟล์​ account อยู่ ให้เรารันคำสั่งด้านล่างเพื่อดูรายการ Account

```bash
geth --datadir . account list
```

## สั่งรัน cmd เปิด private node

1. [Download ไฟล์คำสั่ง](https://www.dropbox.com/s/gafk3ylkmywi9uh/startnode.cmd.zip?dl=0)
2. แตกไฟล์คำสั่งไปไว้ในโฟลเดอร​์ `private`
3. รันคำสั่ง เพื่อเริ่มการทำงานของ Node

```pwsh
.\startnode.cmd
```

## ติดต่อกับ Private Node ผ่าน JavaScript Console

1.  เปิดหน้าต่าง Powershell ใหม่ 

```pwsh
 geth attach ipc:\\.\pipe\geth.ipc
```

2. รันคำสั่งดูข้อมูลใน Private Node

```js
eth.accounts
eth.coinbasee
```

3. ดูจำนวน ether ใน account

```js
eth.getBalance(eth.coinbase)
eth.getBalance(eth.accounts[1])
```


4. แปลงค่า Wei เป็น Ether

```js
web3.fromWei(eth.getBalance(eth.coinbase), "ether");
```

5. สั่งหยุด หรือเริ่ม miner

```bash
miner.stop()
miner.start(1) // with a thread 
```

6. ปลดล๊อค account

```bash
personal.unlockAccount(eth.accounts[1], "1234", 300);
```
### คำอธิบาย Parameter

1. account
2. รหัสผ่าน
3. ปลดล๊อคเป็นเวลา milliseconds หากไม่กำหนดจะเป็็น 10 นาที

## โอนจากบัญชี 1 ไปยังอีกบัญชีหนึ่ง

```bash
eth.sendTransaction({
	from: eth.coinbase,
	to: eth.accounts[1],
	value: web3.toWei(10, "ether")
})
```

### คำอธิบาย Parameter

1. Address ที่จะโอนออก
2. Address ที่จะโอนเข้า
3. จำนวนที่โอน (ใช้ Web 3  แปลงค่า)

ถ้าการทำงานสมบูรณ์ จะมี hash transaction แสดงขึ้นมา สามารถไปดูใน log ของ node ได้ด้วย
