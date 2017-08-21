# Pet Shop Tutorial
Pete Scandlon of Pete's Pet Shop is interested in using Ethereum as an efficient way to handle their pet adoptions. The store has space for 16 pets at a given time, and they already have a database of pets. As an initial proof on concept, Pete wants to see a Dapp which associates an Ethereum address with a pet to be adopted. The website structure and styling will be supplied for you. We need to write the smart contract and front-end logic for its usage.
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