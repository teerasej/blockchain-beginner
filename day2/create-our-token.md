# Deploy My Token

## 1. เปิด Editor

เลือกใช้ Remix Editor หรือ Truffle ตามต้องการ

## 2. สร้างไฟล์ **erc20interface.sol** ใน contracts directory

```
pragma solidity >=0.4.21 <0.9.0;

interface ERC20Interface {
    // uint256 public totSupply;
    
    function totalSupply() external view returns (uint);
    function balanceOf(address tokenOwner) external view returns (uint balance);
    function allowance(address tokenOwner, address spender) external view returns (uint remaining);
    function transfer(address to, uint tokens) external returns (bool success);
    function approve(address spender, uint tokens) external returns (bool success);
    function transferFrom(address from, address to, uint tokens) external returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}
```

3. สร้างไฟล์ **MyCoin.sol** ไว้ใน **contracts** directory

```
pragma solidity >=0.4.21 <0.9.0;

import './erc20Interface.sol';

contract ERC20Token is ERC20Interface {

    uint256 constant private MAX_UINT256 = 2**256 - 1;
    mapping (address => uint256) public balances;
    mapping (address => mapping (address => uint256)) public allowed;

    uint256 public totSupply;           // Total number of tokens
    string public name;                   // Descriptive name (i.e. For Dummies Sample Token)
    uint8 public decimals;                // How many decimals to use when displaying amounts
    string public symbol;                 // Short identifier for token (i.e. FDT)

    // Create the new token and assign initial values, including initial amount
    constructor(
        uint256 _initialAmount,
        string memory _tokenName,
        uint8 _decimalUnits,
        string memory _tokenSymbol
    ) {
        balances[msg.sender] = _initialAmount;              
        totSupply = _initialAmount;                       
        name = _tokenName;                                 
        decimals = _decimalUnits;                        
        symbol = _tokenSymbol;                            
    }

    // Transfer tokens from msg.sender to a specified address
     function  transfer(address _to, uint256 _value) public override returns (bool success)  {
        require(balances[msg.sender] >= _value,"Insufficient funds for transfer source.");
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        emit Transfer(msg.sender, _to, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    // Transfer tokens from one specified address to another specified address
    function transferFrom(address _from, address _to, uint256 _value) public override returns (bool success) {
        uint256 allowance = allowed[_from][msg.sender];
        require(balances[_from] >= _value && allowance >= _value,"Insufficient allowed funds for transfer source.");
        balances[_to] += _value;
        balances[_from] -= _value;
        if (allowance < MAX_UINT256) {
            allowed[_from][msg.sender] -= _value;
        }
        emit Transfer(_from, _to, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    // Return the current balance (in tokens) of a specified address
    function balanceOf(address _owner) public view override returns (uint256 balance) {
        return balances[_owner];
    }

    // Set
    function approve(address _spender, uint256 _value) public  override returns (bool success) {
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    // Return the
    function allowance(address _owner, address _spender) public view override returns (uint256 remaining) {
        return allowed[_owner][_spender];
    }

    // Return the total number of tokens in circulation
    function totalSupply() public view override returns (uint256 totSupp) {
        return totSupply;
    }
}
```



## 4. ทำการ Deploy 

### ถ้าเราใช้ Remix 

สามารถสั่ง Deploy และกรอก parameter ได้ตามภาพ

<img width="360" alt="Screen Shot 2565-05-11 at 09 10 47" src="https://user-images.githubusercontent.com/85179/167754948-718340de-4d30-461d-91a9-68397d86924c.png">


### ถ้าเราใช้ Truffle 

ให้สร้าง migration ไฟล์ และรันคำสั่ง `truffle deploy`

```js
const PonCoin = artifacts.require("PonCoin");
module.exports = function (deployer) {
  deployer.deploy(PonCoin,1000000,"PonCoin", 18, "PCOIN");
};
```