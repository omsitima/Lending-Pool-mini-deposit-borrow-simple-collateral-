# Lending-Pool-mini-deposit-borrow-simple-collateral-
Lending Pool mini (deposit, borrow, simple collateral)
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

contract MiniLending {
    IERC20 public token;
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrows;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint256 amount) public {
        token.transferFrom(msg.sender, address(this), amount);
        deposits[msg.sender] += amount;
    }

    function borrow(uint256 amount) public {
        require(deposits[msg.sender] >= amount * 2, "Need 200% collateral");
        borrows[msg.sender] += amount;
        token.transfer(msg.sender, amount);
    }

    function repay(uint256 amount) public {
        token.transferFrom(msg.sender, address(this), amount);
        borrows[msg.sender] -= amount;
    }
}
