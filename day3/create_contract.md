# สร้าง Dapps

- boxes เป็น template สำหรับสร้าง Web frontend 
- สามารถดู Box สำเร็จรูปได้ที่ [https://www.trufflesuite.com/boxes](https://www.trufflesuite.com/boxes)

## 1. สร้าง Smart Contract

1. สร้างโฟลเดอร์ `training/shopper`
2. ใช้ terminal เปิดมาที่ `training/shopper`
3. รันคำสั่ง  

```pwsh
truffle unboxes teerasej/shopper-box
```

4. เปิดโฟลเดอร์ `contracts`
5. สร้างไฟล์ `ChainList.sol`
6. เขียน Contract ด้านล่าง

```js
pragma solidity ^0.5.8;

contract ChainList {
    
    address buyer;
    address payable seller;
    string name;
    string description;
    uint price;

	// Sell thing info
    function sellArticle(string memory _name, string memory _description, uint _price) public {
        seller = msg.sender;
        buyer = address(0x0);
        name = _name;
        description = _description;
        price = _price;
    }

	// get thing info
    function getArticle() public view returns (
        address _seller,
        address _buyer,
        string memory _name,
        string memory _description,
        uint _price
    ) {

        return (seller, buyer, name, description, price);
    }

}
```

- **msg** เป็น object ที่คืนค่าสำคัญต่างๆ ให้เราได้ เช่น `msg.sender` จะเป็น address ของผู้เรียกใช้ Smart Contract นี้ 
- **unit256** ไว้เก็บค่า Wei ซึ่งเราใช้ Type นี้ในการเก็บค่า Wei บ่อยๆ
- **view** เป็น modifier keyword ที่หมายถึง function ดังกล่าวจะไม่มีการเปลี่ยนแปลงค่าใน Contract (ไม่มีการเสียค่า Gas และไม่เกิด Transaction)

7. บันทึกไฟล์
8. แก้ไขไฟล์ `Migration.sol` ให้เป็นเวอร์ชั่น 0.5.8

```js
pragma solidity ^0.5.8;

contract Migrations {
  address public owner;
  uint public last_completed_migration;

  modifier restricted() {
    if (msg.sender == owner) _;
  }

  constructor() public {
    owner = msg.sender;
  }

  function setCompleted(uint completed) public restricted {
    last_completed_migration = completed;
  }

  function upgrade(address new_address) public restricted {
    Migrations upgraded = Migrations(new_address);
    upgraded.setCompleted(last_completed_migration);
  }
}

```

## 2. สร้างคำสั่ง Migration

8. เปิดโฟลเดอร์ `migrations`
9. สร้างไฟล์ `2_deploy_contract.js`

```js
var ChainList = artifacts.require("./ChainList.sol");

module.exports = function(deployer) {
	deployer.deploy(ChainList);
}
```

10. บันทึกไฟล์


## 3. Deploy

1. เปิด Ganache 
2. เปิด Terminal ในโปรเจค GreetingTruffle 
3. รันคำสั่ง `truffle migrate --network ganache` ใน terminal
4. หลัง Deploy Smart contract แล้ว รันคำสั่ง `truffle console --network ganache`
5. ทดสอบรันคำสั่ง `ChainList.address` จะเห็น Address ของ Contract
6. ลองเปิดโฟลเดอร์ `chainlist\build` และเปิดไฟล์ `ChainList.json` 
7. ลงมาด้านล่าง จะเห็นส่วน **Network** ที่ใช้ ID ของ Ganache 

## 4. ทดสอบใช้งาน

1. กลับมาที่ Console
2. รันคำสั่งเช็คค่า eth ใน 2 บัญชี

จะเห็นว่าบัญชีแรกเสีย eth ไปสำหรับการ deploy Smart Contract

```js
account0 = (await web3.eth.getAccounts())[0]
balance0 = await web3.eth.getBalance(account0)
web3.utils.fromWei(balance0,'ether')

account1 = (await web3.eth.getAccounts())[1]
balance1 = await web3.eth.getBalance(account1)
web3.utils.fromWei(balance1,'ether')
```

3. รันคำสั่งเพื่ออ้างถึง Smart Contract และดู function ของ Smart Contract

```js
ChainList.deployed().then(function(instance) {app = instance})

app
```

4. ทดสอบดูค่าใน Smart Contract

```js
app.getArticle()
```

จะเห็นว่าค่าเป็น `‘’` เพราะยังไม่มีการใช้งาน Smart Contract

5. สร้าง Article ด้วยคำสั่งด้านล่าง (สังเกตว่าเราใช้ Account 2 เป็นตัวสร้าง Contract)

```js
account1 = (await web3.eth.getAccounts())[1]

price = web3.utils.toWei('3','ether')

app.sellArticle("iPhone Xs", "Selling for iPhone Xs. 2", price, { from: account1 })
```

เราจะเห็นการสร้าง transaction 

6. ใช้คำสั่ง เรียกดูข้อมูล

```js
app.getArticle()
```

7. เรียกดูค่า eth ใน account ทั้ง 2 อีกครั้ง

```js
account0 = (await web3.eth.getAccounts())[0]
balance0 = await web3.eth.getBalance(account0)
web3.utils.fromWei(balance0,'ether')

account1 = (await web3.eth.getAccounts())[1]
balance1 = await web3.eth.getBalance(account1)
web3.utils.fromWei(balance1,'ether')
```


