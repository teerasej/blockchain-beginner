
# สร้าง client 

## 1. Clone โปรเจค

ทำการโคลนโปรเจคมาจาก [repo](https://github.com/teerasej/nextflow-nextjs-web3-event-simple)

```
git clone https://github.com/teerasej/nextflow-nextjs-web3-event-simple --branch start
```

ทำการรันคำสั่งติดตั้ง package ตามที่ต้องการ

```
npm i 

หรือ

yarn add
```

## 2. สร้างไฟล์ environment 

สร้างไฟล์ **.dev.local** ขึ้นมาในโฟลเดอร์โปรเจค และทำการใส่ Smart contract address ที่ได้มาลงไป

```
NEXT_PUBLIC_EVENT_CONTRACT_ADDRESS=<Smart contract address>
```

## 3. นำ artifact ของ Smart contract มาไว้ในโปรเจค

นำ artifact ของ Smart contract มาไว้ใน **contracts** directory 

## 4. รันคำสั่ง build และรันทดสอบแอพพลิเคชั่น

รันคำสั่งใน terminal

```
npm run build
npm run dev
```

คำสั่งที่ 2 จะเป็นการรัน server ขึ้นมาที่ http://localhost:3000

