# Level 3: Coin Flip

## Goal
Guess the coin flip correctly 10 times in a row.

## The bug
The contract creates its "random" result from `blockhash(block.number - 1)`. Block data is public, so anyone can calculate the same result before calling `flip`. It also only allows one flip per block, which does not stop a contract from calculating the answer.

## How I attacked it
1. Wrote a `CoinFlipAttack` contract that copies the level's calculation: previous block hash divided by the same `FACTOR`.
2. Deployed it on Sepolia using Remix, with the level's address.
3. Called `attack()` once per block until `consecutiveWins` reached 10.

## The fix
Never use block data (`blockhash`, `timestamp`, `prevrandao`) as a source of randomness when money depends on it. Use a verifiable randomness oracle such as Chainlink VRF.

## What I learned
Anything a contract can read on-chain, an attacker's contract can read and predict too.