# Level 6: Delegation

## Goal
Claim ownership of the Delegation contract.

## The bug
The `fallback` function in Delegation forwards any call it does not recognize to another contract (Delegate) with `delegatecall`. `delegatecall` runs the other contract's code using the caller's storage. Both contracts store `owner` in the first storage slot, so when Delegate's `pwn()` sets `owner = msg.sender`, it changes the owner of Delegation.

## How I attacked it
1. Sent a transaction to Delegation with the data for `pwn()`.
2. Delegation did not have a `pwn()` function, so the fallback ran.
3. The fallback sent the call to Delegate with `delegatecall`.
4. `pwn()` ran in Delegation's storage and made me the owner.

## The fix
Do not forward unknown calls to another contract with `delegatecall` unless the call data is controlled. Make sure the storage layout of both contracts is safe, and limit which functions can be called.

## What I learned
`delegatecall` runs someone else's code with my contract's storage. Anyone who can control what is called can overwrite my state, including `owner`.
