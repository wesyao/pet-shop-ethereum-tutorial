# Pet Shop Tutorial
Pete Scandlon of Pete's Pet Shop is interested in using Ethereum as an efficient way to handle their pet adoptions. The store has space for 16 pets at a given time, and they already have a database of pets. As an initial proof on concept, Pete wants to see a Dapp which associates an Ethereum address with a pet to be adopted. The website structure and styling will be supplied for you. We need to write the smart contract and front-end logic for its usage.

http://truffleframework.com/tutorials/pet-shop

## Dependencies
- Node v6+
- Git

## Setup
```javascript
npm install -g ethereumjs-testrpc
npm install -g truffle
```

## Create Truffle Project
```javascript
// Create the directory.
mkdir pet-shop-tutorial

// Navigate to within the directory.
cd pet-shop-tutorial

// Initialize Truffle with the base pet shop code
truffle unbox pet-shop
```

## Writing a Smart Contract
- Create Adoption.sol
- The minimum version of solidity that is required is noted at the top of the contract. The ^ means this version or newer.
```javascript
pragma solidity ^0.4.4;

contract Adoption {
    // code here
}
```

- Lines are terminated by semicolons.

### Variable Setup
- Solidity is a static type language.
- **address** is a unique datatype that are Ethereum addresses. It is stored as 20 byte values. Every account and smart contract on the Ethereum blockchain has an address and can send/receive Ether from/to this address

Now create an addresses array of size 16
```javascript
address[16] public adopters;
```
- adropters is an array of Ethereum addresses
- arrays can only contain 1 type and have fixed lengths
- **public** variables have automatic getter methods, but in the case of arrays a key is required and will only return a single value

### First Function: Adopt a Pet
Lets allow users to make adoption requests.

```javascript
function adopt(uint petId) public returns (uint) {
  require(petId >= 0 && petId <= 15);

  adopters[petId] = msg.sender;

  return petId;
}
```
- Solidity must have types for inputs and outputs defined
- **msg.sender** will give you the address or smart contract who called this function

### Second Function: Retrieving the Adopters
