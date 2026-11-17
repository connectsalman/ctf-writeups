# Level 5: Token

## Goal
Start with 20 tokens and get more than 20.

## The bug
The contract uses an old Solidity version (before 0.8) with no overflow protection. The `transfer` function checks `require(balances[msg.sender] - _value >= 0)`. Because `uint` can never be negative, this check always passes, and `balances[msg.sender] - _value` wraps around to a huge number when `_value` is bigger than the balance.

## How I attacked it
1. Called `transfer(contract.address, 21)` with a balance of 20.
2. `20 - 21` underflowed to a huge number.
3. My balance became a very large number.

## The fix
Use Solidity 0.8 or newer, which reverts on overflow and underflow. For old code, use OpenZeppelin's SafeMath. Also check `balances[msg.sender] >= _value` directly.

## What I learned
In old Solidity, math can wrap around. A check like `x - y >= 0` on unsigned numbers is always true, so it protects nothing.
