# ส่ง Eth 

. เปิด Ganache 
2. เปิด Terminal ในโปรเจค GreetingTruffle 
3. รันคำสั่ง `truffle migrate --compile-all --reset --network ganache` ใน terminal
4. หลัง Deploy Smart contract แล้ว รันคำสั่ง `truffle console --network ganache`
5. รันคำสั่งเพื่อส่งค่าจาก Acccount 1 ไปยัง Account 2 

```js
web3.eth.sendTransaction(
   {
		from: web3.eth.accounts[0],
   		to: web3.eth.accounts[1], 
		value: web3.toWei(5,"ether")
	}
)
```

เราจะได้ transaction hash มา ให้เช็คจำนวนใน account 1 และ 2 จาก Ganache และดูรายละเอียดใน Transaction 

## เช็ค Gas ใน Main network

[https://ethgasstation.info/](https://ethgasstation.info/)