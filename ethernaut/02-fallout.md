# Level 2: Fallout

## Goal
Claim ownership of the contract.

## The bug
The function meant to be the constructor is named `Fal1out` (with the number 1), but the contract is named `Fallout` (with the letter l). Because the names don't match, it is not a constructor. It is a normal public function that anyone can call at any time, and it sets `owner` to the caller.

## How I attacked it
Called `Fal1out()` from the console. I became the owner immediately.

## The fix
Use the `constructor` keyword. Modern Solidity does not allow name-based constructors, so this bug can't happen in new code.

## What I learned
A small typo can become a critical bug. Always check that initialization code really runs only once, at deployment.