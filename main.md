    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.17;

    contract Declaration{

        address owner;
        uint Quantity;
        string name;
        string symbol;

        mapping (address => uint) public Balances;
        mapping (address => uint) public Approve;
        
        constructor(string memory Name , string memory Symbol){
        owner = msg.sender;
        Quantity = 10000;
        Balances[owner] = Quantity;
        name = Name;
        symbol = Symbol;
        }

    event TransferInfo(address _addr , uint _quantity);
    event Approvetransfer(address Owner , address to , uint _quantity);

    modifier onlyowner(){
    require( msg.sender == owner , "Only Owner Can be Call this Function");
        _;
    }

    function CheckBalance(address _Addr) view  external returns(uint Balance){
    return Balances[_Addr];
    }

    function Transfer(address to , uint quantity) external onlyowner returns(bool){
    require(Balances[owner] > quantity , "insufficient balance");
    require(to != owner , "Invalid address" );

    Balances[owner] -= quantity;
    Balances[to] += quantity;

    emit TransferInfo(owner, quantity);
    return  true;
    }

    function Setapprove(address manager , uint quantity) external onlyowner returns(bool){
        Approve[manager] = quantity;
        return true;
    }

    function ApproveTransfer(address From , address to , uint quantity) external returns (bool){
        require(Balances[owner] > quantity , "insufficient balance");
        require(Approve[msg.sender] > quantity , "insufficient balance");
        Balances[From] -= quantity;
        Balances[to] += quantity;
        Approve[msg.sender] -= quantity;
        emit Approvetransfer(From, to, quantity);
        return true;
    }

    function CheckApproveBalance(address _addr) view external returns(uint){
    return Approve[_addr];
    }
    }
