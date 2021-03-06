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
Array getters only return a single value for a given key. We need a helper function for getting all adopters.

```javascript
function getAdopters() public returns (address[16]) {
  return adopters;
}
```

### Compiling and Migrating the Smart Contract

#### Compiling
- Solidity is a compiled language. We need to compile the code for EVM (Ethereum Virtual Machine) to run.

Open a new terminal and run testrpc. This will start a new local blockchain. This will provide you a set of accounts, keys, and a wallet.

```javascript
testrpc
EthereumJS TestRPC v4.0.1 (ganache-core: 1.0.1)

Available Accounts
==================
(0) 0x996f29dd606837fe947ba3864e49c72a6e146685
(1) 0x6aba9e3dfaa8e7fd909689b4fb1b09aff0ca2739
(2) 0xef0ee327a69815e9aafc0bcb73c21586ecbb382d
(3) 0x92b70c81494f8e9dab14a29821ce61ecbdf87cd0
(4) 0x4c95d97b4dfb8bcac3d78f74761c4208364cd7eb
(5) 0x22b9cdbadd5a320ecaddeb01b6bb8881b275b8d9
(6) 0xcb7d3c014567554a2bdd495d999d941b2f5583a9
(7) 0x5a65615b53fa77029276ca1c01b87ced9d32163a
(8) 0xb1c1b72a08a033d3ebf9a62614965b39196ee4f5
(9) 0xc341f64bf61d4daf177429f9029c4cf40056d417

Private Keys
==================
(0) 0b94f7327280dfbad7f1974e27a4532dffecda795a6d9d30f4fc81823976f7e8
(1) 4e16391345babd7423567ab7aaf89b27f75aaa8a186287aca5fdcf8bc2a3dcb0
(2) dc333176e89d4f646e64bddf01bcf261350cfed0d4fac4fe0e9b9a43b7799839
(3) 132171fa63c70db3496af5c1c2cbb2eb70bf737ef035e8d8f6ee44b77ec23b72
(4) 49c62668eef171a0019c0b4a412492682f67a4342c87abf8cbcc061d5f1a138c
(5) d77e04c89ba4dc0f2079221e9127dd8023001c273b3b503b9b23dcbdd91555ff
(6) db54f99c64a03caca275a265b83209a26bc716e4fe1d99be97e4822177adc982
(7) 96976c5f43c31497e70c6da8b02f0ebf7ca76888d1444cb2854af3d82381f3b3
(8) 5db119ca5de792edcebb664052f49c3a6eceb6d0674d38ffdcccf81e5f2bb3d6
(9) c392a921850ef910c10446575bdc8d467ee3892e88d0880a3b58a8cf696bfa14

HD Wallet
==================
Mnemonic:      intact boat prison yard order agent render loud forward cost fashion front
Base HD Path:  m/44'/60'/0'/0/{account_index}
```

Compile your contract in another terminal.
```javascript
truffle compile
```

#### Migration
- **migration** is a deployment script meant to alter the state of your application's contracts, moving it from one state to the next. This may be deploying new code or even replacing contracts.

- `1_initial_migration.js.` handles deploying the Migrations.sol contract to observe subsequent smart contract migrations.

Migrations are executed in the order specified and follow the same basic structure:

- Import the desired contract artifacts from the build folder.
- Export a single, anonymous function taking one argument, `deployer`.
- Order the deployment of a given contract with `deployer.deploy(<< CONTRACT_NAME >>)`.

##### Create Adoption Migration
Create `2_deploy_contracts.js` in `migrations` directory.

```javascript
var Adoption = artifacts.require("./Adoption.sol");

module.exports = function(deployer) {
  deployer.deploy(Adoption);
};
```

run `truffle migrate`

Notice the migrations are ran in order followed by the addresses of the deployed contracts

```javascript
Using network 'development'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0xe5d04056e4eefb2290ccc6bb4e08e278a529064d94c64aac930f0f9dcf7e341f
  Migrations: 0x97d1311a040e8efffff834abbad746daefbd04e5
Saving successful migration to network...
  ... 0xdb4dda88d726456a75383d29f2bcc4f5b958f88db15d14c547f92f5cb7171492
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying Adoption...
  ... 0x8ab3559b156479b2c743325ed1d42583f9aa27f452304b0979e603465a52fcbb
  Adoption: 0xdf02daf5124e6bded7e21de155163d98bf8563fd
Saving successful migration to network...
  ... 0xf8608252306749e41973dfdb4de517e030cb297ac13877493db1014ed31dd1f8
Saving artifacts...
```

### Testing the Smart Contract

Create `TestAdoption.sol` under the `test` directory

```javascript
pragma solidity ^0.4.11;

import "truffle/Assert.sol";
import "truffle/DeployedAddresses.sol";
import "../contracts/Adoption.sol";

contract TestAdoption {
  Adoption adoption = Adoption(DeployedAddresses.Adoption());
  //Tests go here
}
```
We import 3 things:
- `Assert.sol`: gives us various assertions to use in our tests. In testing, an assertion checks for things like equality, inequality or emptiness to return a pass/fail boolean from our test.
- `DeployedAddresses.sol`: When running tests, Truffle will deploy a fresh instance of the contract being tested to the TestRPC. This smart contract gets the address of the deployed contract.
- The smart contract we want to test (Adoption.sol).

#### Testing adopt()

Test a function on a smart contract by writing a function and checking an assertion.
```javascript
function testUserCanAdoptPet() {
  uint returnedId = adoption.adopt(8);

  uint expected = 8;

  // testedValue, expectedValue, error message if failure
  Assert.equal(returnedId, expected, "Adoption of pet ID 8 should be recorded.");
}
```

Data will persist throughout the entire life of the test. We can test the getter for the adoption variable.

Since TestAdoption will be sending the transaction, we set the value to `this`. This is because `this` is a contract wide variable that gets the current contract's address.

```javascript
function testGetAdopterAddressByPetId() {
  address expected = this;

  address adopter = adoption.adopters(8);

  Assert.equal(adopter, expected, "Owner of pet ID 8 should be recorded.");
}
```

#### Testing getAdopters()

`public` provides automatic getters. However, solidity arrays can only provide a single value per key. This is why we wrote getAdopters().

`memory` property tells solidity to store the value in memory instead of contract storage.
```javascript
function testGetAdopterAddressByPetIdInArray() {
  address expected = this;

  address[16] memory adopters = adoption.getAdopters();

  Assert.equal(adopters[8], expected, "Owner of pet ID 8 should be recorded.");
}
```

#### Run Tests

Run `truffle test`

## Creating UI to interact with Smart Contract
Up to this point we:
1. built the smart contract
2. built & migrated the contract to our local blockchain
3. interacted with the blockchain through tests

Now we need to create an UI to interact with the smart contract.

### Instantiating Web3
Open `app.js` and notice we initialize the app from the `init` function. `init` also calls `initWeb3`.

Web3 is a JavaScript library for interacting with the Ethereum blockchain
Etherum browsers like [Mist](https://github.com/ethereum/mist) or chrome extension [MetaMask](https://metamask.io/) will inject their own instance of web3.

This is not suitable for production.

Remove the comments and replace with
```javascript
// Initialize web3 and set the provider to the testRPC.
if (typeof web3 !== 'undefined') {
  App.web3Provider = web3.currentProvider;
  web3 = new Web3(web3.currentProvider);
} else {
  // set the provider you want from Web3.providers
  App.web3Provider = new web3.providers.HttpProvider('http://localhost:8545');
  web3 = new Web3(App.web3Provider);
}
```

### Instantiate Contract
We need to instantiate our contract so that `web3` knows where to find the contract. Truffle has a nice library for this called `truffle-contract`. This keeps the information in sync during migration.

Take another look at `initContract` inside `app.js`. Replace the comments with this block.

What this is doing is `artifact` file (Adoption.json) for our contract. `Artifacts` contain information about the contract such as deployed addresses and `ABI` (Application Binary Interface). The `ABI` contains information about variables and functions of your contract.

```javascript
$.getJSON('Adoption.json', function(data) {
  // Get the necessary contract artifact file and instantiate it with truffle-contract.
  var AdoptionArtifact = data;
  App.contracts.Adoption = TruffleContract(AdoptionArtifact);

  // Set the provider for our contract.
  App.contracts.Adoption.setProvider(App.web3Provider);

  // Use our contract to retieve and mark the adopted pets.
  return App.markAdopted();
});
```
