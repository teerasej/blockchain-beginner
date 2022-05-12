# สร้าง Smart Contract

หากยังไม่มี truffle project ให้สร้างตามขั้นตอนปกติก่อน เช่น

## สร้าง truffle project

1. สร้างโฟลเดอร์ **my-event-contract**
2. รันคำสั่ง `truffle init` ภายในโฟลเดอร์

## 1. สร้าง Smart Contract

1. สร้างไฟล์ `EventContract.sol` ในโฟลเดอร์ **my-event-contract/contracts**

```solidity
pragma solidity >=0.4.21 <0.9.0;

contract EventContract {
    uint256 public event_id = 0;

    struct anEvent {
        string name;
        string description;
        address eventOwner;
    }

    mapping(uint256 => anEvent) public events;

    function createNewEvent(string memory _name, string memory _description ) public returns (uint256) {
        uint256 eid = event_id++;
        events[eid].name = _name;
        events[eid].description = _description;
        events[eid].eventOwner = msg.sender;
        return eid;
    }


    function getEventCount() public view returns(uint256){
        return event_id;
    }

    function getEventInformation(uint256 eventId) public view returns (string memory, string memory, address) {
        anEvent memory targetEvent = events[eventId];

        return (targetEvent.name, targetEvent.description, targetEvent.eventOwner);
    }
 
}
```


## 2. สร้างคำสั่ง Migration

1. เปิดโฟลเดอร์ `migrations`
2. สร้างไฟล์ `2_deployEventContract.js`

```js
const EventContract = artifacts.require("EventContract");
module.exports = function (deployer) {
    deployer.deploy(EventContract);
};
```

3. รันคำสั่ง deploy project

```
truffle deploy --reset
```


## 3. เก็บข้อมูลที่ต้องใช้ใน client

1. คัดลอกไฟล์ build/contracts/EventContract.json
2. คัดลอก Contract Address ที่ได้จากการ deploy