# Level 1: Fallback

## Goal
Become the owner of the contract and withdraw all its ETH.

## The bug
The `receive()` function makes `msg.sender` the owner. Its only check is that the caller has contributed something before. Anyone can pass that check with a tiny contribution.

## How I attacked it
1. Called `contribute()` with 0.0005 ETH to get a contribution above 0.
2. Sent 0.0005 ETH directly to the contract, which triggered `receive()` and made me the owner.
3. Called `withdraw()` to take all the ETH.

## The fix
Never change ownership in `receive()` or `fallback()`. Ownership changes should go through a dedicated function that only the current owner can call.

## What I learned
Special functions like `receive()` are part of the attack surface. Always check what they can change.