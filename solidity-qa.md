# Solidity Interview Questions & Answers

## What is Solidity?
Solidity is a statically-typed, contract-oriented programming language designed for writing smart contracts on the Ethereum blockchain and other EVM-compatible networks.

## What is the difference between a contract and an account in Solidity?
An account can be externally owned (controlled by a private key) or a contract account (controlled by code). A contract is code deployed on the blockchain that executes when called.

## What are the visibility modifiers in Solidity?
- **public**: Accessible from anywhere (internal and external)
- **private**: Only accessible within the defining contract
- **internal**: Accessible within the contract and derived contracts
- **external**: Only callable from outside the contract

## What is the difference between `memory`, `storage`, and `calldata`?
- **storage**: Persists on-chain between function calls, most expensive to use
- **memory**: Temporary data that exists only during function execution
- **calldata**: Read-only data area for external function arguments, cheaper than memory

## What are modifiers in Solidity?
Modifiers are reusable code blocks that run before or after a function executes. They are commonly used for access control, input validation, and pre/post conditions.

## What is the difference between `view` and `pure` functions?
- **view**: Reads state but does not modify it (no gas cost when called externally)
- **pure**: Neither reads nor modifies state, only uses its parameters and local variables

## What is a fallback function?
A fallback function is executed when a contract is called with no matching function signature or no data. It has no name, no arguments, and no return value. Declared as `fallback() external payable`.

## What is a receive function?
A receive function is executed when a contract receives plain Ether with no calldata. Declared as `receive() external payable`. A contract can have at most one receive function.

## What is reentrancy and how do you prevent it?
Reentrancy occurs when an external contract calls back into your contract before the first invocation finishes. Prevent it using the Checks-Effects-Interactions pattern, reentrancy guards (`nonReentrant` modifier), or pull payment patterns.

## What is the difference between `transfer`, `send`, and `call` for sending Ether?
- **transfer**: Reverts on failure, limited to 2300 gas, deprecated in newer Solidity versions
- **send**: Returns false on failure, limited to 2300 gas, also deprecated
- **call**: Returns success status and data, forwards all gas by default, recommended approach using `address.call{value: amount}("")`

## What are events and why are they useful?
Events allow logging data to the blockchain's event log, which is stored off-chain but indexed for efficient querying. They are useful for front-end notifications, debugging, and recording historical state changes cheaply.

## How does inheritance work in Solidity?
Solidity supports multiple inheritance using the `is` keyword. Contracts can inherit state variables, functions, and modifiers from base contracts. Virtual and override keywords are required when overriding functions.

## What is `delegatecall` and how does it differ from `call`?
`delegatecall` executes code from another contract but in the context of the calling contract — meaning it uses the caller's storage, balance, and address. It is commonly used for proxy patterns and upgradeable contracts.

## What are custom errors and when should you use them?
Custom errors (introduced in Solidity 0.8.4) are defined with the `error` keyword and revert with `revert MyError()`. They are more gas-efficient and descriptive than `require` string messages.

## What is the purpose of interfaces and abstract contracts?
**Interfaces** declare function signatures without implementation and cannot have state variables or constructors. **Abstract contracts** can have partial implementation, state variables, and unimplemented functions. Both define contracts that others must conform to.
