# สร้าง Dapps

- boxes เป็น template สำหรับสร้าง  web frontend 


## สร้าง Smart Contract

1. สร้างโฟลเดอร์ `training/chainlist`
2. ใช้ terminal เปิดมาที่ `training/chainlist`
3. รันคำสั่ง  

```pwsh
truffle unboxes chainskills/chainskills-box
```

4. เปิดโฟลเดอร์ `contracts`
5. สร้างไฟล์ `ChainList.sol`
6. เขียน Contract ด้านล่าง

```js
//solium-disable linebreak-style
pragma solidity ^0.4.18;

contract ChainList {
    
    address buyer;
    address seller;
    string name;
    string description;
    uint256 price;

	// Sell thing info
    function sellArticle(string _name, string _description, uint256 _price) public {
        seller = msg.sender;
        name = _name;
        description = _description;
        price = _price;
    }

	// get thing info
    function getArticle() public view returns (
        address _seller,
        address _buyer,
        string _name,
        string _description,
        uint256 _price
    ) {

        return (seller, buyer, name, description, price);
    }

}
```

- **msg** เป็น object ที่คืนค่าสำคัญต่างๆ ให้เราได้ เช่น `msg.sender` จะเป็น address ของผู้เรียกใช้ Smart Contract นี้ 
- **unit256** ไว้เก็บค่า Wei ซึ่งเราใช้ Type นี้ในการเก็บค่า Wei บ่อยๆ
- **view** เป็น modifier keyword ที่หมายถึง function ดังกล่าวจะไม่มีการเปลี่ยนแปลงค่าใน Contract (ไม่มีการเสียค่า Gas และไม่เกิด Transaction)

7. บันทึกไฟล์
8. เปิดโฟลเดอร์ `migrations`
9. สร้างไฟล์ `2_deploy_contract.js`

```js
var ChainList = artifacts.require("./ChainList.sol");

module.exports = function(deployer) {
	deployer.deploy(ChainList);
}
```

10. บันทึกไฟล์


## Deploy

1. เปิด Ganache 
2. เปิด Terminal ในโปรเจค GreetingTruffle 
3. รันคำสั่ง `truffle migrate --network ganache` ใน terminal
4. หลัง Deploy Smart contract แล้ว รันคำสั่ง `truffle console --network ganache`
5. ทดสอบรันคำสั่ง `ChainList.address` จะเห็น Address ของ Contract
6. ลองเปิดโฟลเดอร์ `chainlist\build` และเปิดไฟล์ `ChainList.json` ลงมาด้านล่าง จะเห็นส่วน **Network** ที่ใช้ ID ของ Ganache 

## ทดสอบใช้งาน

1. กลับมาที่ Console
2. รันคำสั่งเช็คค่า eth ใน 2 บัญชี

จะเห็นว่าบัญชีแรกเสีย eth ไปสำหรับการ deploy Smart Contract

```js

web3.fromWei(web3.eth.getBalance(web3.eth.accounts[0])).toNumber()

web3.fromWei(web3.eth.getBalance(web3.eth.accounts[1])).toNumber()
```

3. รันคำสั่งเพื่ออ้างถึง Smart Contract และดู function ของ Smart Contract

```js
> Greetings.deployed().then(function(instance) {app = instance})

> app
```

4. ทดสอบดูค่าใน Smart Contract

```js
app.getArticle()
```

จะเห็นว่าค่าเป็น `‘’` เพราะยังไม่มีการใช้งาน Smart Contract

5. สร้าง Article ด้วยคำสั่งด้านล่าง (สังเกตว่าเราใช้ Account 2 เป็นตัวสร้าง Contract)

```js
app.sellArticle(
	"iPhone Xs",  
	"Selling for iPhone Xs. 2", 
	web3.toWei(3, "ether"),
	{ from: web3.eth.accounts[1] } 
)
```

เราจะเห็นการสร้าง transaction 

6. ใช้คำสั่ง เรียกดูข้อมูล

```js
app.getArticle()
```

7. เรียกดูค่า eth ใน account ทั้ง 2 อีกครั้ง

```js

web3.fromWei(web3.eth.getBalance(web3.eth.accounts[0])).toNumber()

web3.fromWei(web3.eth.getBalance(web3.eth.accounts[1])).toNumber()
```


