
# Data Type 

สร้างไฟล์ **DataType.sol** ใน **contracts** directory

```
pragma solidity ^0.8.13;

contract DataTypes {
    uint256 x = 23;
    int256 i = -68;
    uint8 j = 17;
    bool isMoon = true;
    address owner = msg.sender;

    bytes32 bMsg = "hello";
    string sMsg = "hello";

    function getStateVariable()
        public
        view
        returns (
            uint256,
            int256,
            uint8,
            bool,
            address,
            bytes32,
            string memory
        )
    {
        return (x, i, j, isMoon, owner, bMsg, sMsg);
    }

    function setbMsg(bytes32 newValue) private returns (bytes32) {
        bMsg = newValue;
    }

    function iWillNotTouchBlockchain() public view {}
}
```

## 2. เขียนคำสั่ง migrate

ทดสอบ Deploy Smart Contract โดยการสร้างไฟล์ deploy ไว้ใน **migrations** directory

```js
// 3_deployDataType.js

const DataType = artifacts.require("DataType");

module.exports = function (deployer) {
  deployer.deploy(DataType);
};
```

## 3. รันคำสั่ง deploy

```
truffle deploy --reset
```