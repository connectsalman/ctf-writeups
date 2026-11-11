# Level 4: Telephone

## Goal
Claim ownership of the contract.

## The bug
`changeOwner` only works if `tx.origin != msg.sender`. `tx.origin` is the wallet that started the transaction, and `msg.sender` is whoever called the function directly. A middle contract makes the two different, so the check can be passed on purpose.

## How I attacked it
1. Wrote an `Attack` contract that calls `changeOwner` on Telephone.
2. Deployed it from my wallet using Remix.
3. Called `attack(telephone, myWallet)`. Telephone saw `msg.sender` as the Attack contract and `tx.origin` as my wallet, so the check passed and I became the owner.

## The fix
Never use `tx.origin` for authorization. Check `msg.sender` instead.

## What I learned
The same `tx.origin` mistake lets attackers steal funds in phishing attacks, which I also built in my vulnerabilities-poc repo.