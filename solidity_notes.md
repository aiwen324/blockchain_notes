- [Data Location](#data-location)
- [Tags (Variables)](#tags-variables)
- [Tags (Methods)](#tags-methods)
- [Special Functions](#special-functions)
- [ABI](#abi)
- [Tools and Framework](#tools-and-framework)
  - [Ganache and Truffle Relationship](#ganache-and-truffle-relationship)
  - [Truffle](#truffle)
  - [Truffle Console](#truffle-console)
  - [Bugs:](#bugs)
  - [Web3](#web3)

## Data Location

- Memory
- Storage
- Calldata

## Tags (Variables)

- `public`: The compiler automatically generates the getter method.
- `private`: The variable is only editable by the smart contract itself, and not even to the child contracts.
- `internal`: This can be used if you want the state variable to be editable to the smart contract and its child contracts

## Tags (Methods)

- `internal`: can only be called from the current contract or inherited ones
- `external`: specified to be called by an external account or contract. External methods consist of an address and a method signature, and they can be passed via and returned from external method calls.
- `public`: can be treated as `internal` and `external`.
- `private`: only visible to the current contract **(not even to the child contracts)**
- `modifier`: inheritable properties of contracts. Following is an example of `modifier`
- `view`: The `view` keyword in the function declaration means that the function will not modify the state of the contract

  `onlyOwner` check the user of the method with such tag be the owner of the contract

  ```js
  function setGreeting(string memory _greeting) public onlyOwner {
      greeting = _greeting;
  }
  // ...
  modifier onlyOwner() {
      require(
          msg.sender == _owner,
          "Ownable: caller is not he owner"
      );
      _;
  }
  ```

## Special Functions

Check this link and following sample code: https://docs.soliditylang.org/en/v0.8.9/contracts.html?highlight=fallback#fallback-function

## ABI

In Ethereum, the ABI is used to encode contract calls for the EVM and to read data out of transactions. The purpose of an ABI is to define the functions in the contract that
can be invoked and describe how each function will accept arguments and return its result.

## Tools and Framework

### Ganache and Truffle Relationship

Check https://ethereum.stackexchange.com/questions/58093/difference-between-ganache-and-truffle/58096

**Notes**: Truffle will use remote (online) compiler (surprised right? I was when I saw those IPFS address)

### Truffle

In the `truffle-config.js`, to use event listener, you need to enable `websockets: true` whereas its default is `False`.

### Truffle Console

Truffle Console will wrap up a lot of things.

- `truffle-contract`
- `web3`
- `artifacts` (From Truffle): `artifacts` is a JSON bundle that contains lots of information (including [ABI](#abi)). While in console, we don't have to do contract intialization. In the js code, we need to initialize contract with `truffle-contract`, then load `truffle artifacts` (contractJson) to the contract.

### Bugs:

- JSON Error: Truffle migrate might cause `SyntaxError: Unexpected end of JSON input`. This is caused by corrputed `build/contract`

- How to deploy one address after another: https://ethereum.stackexchange.com/questions/18432/truffle-migrate-fails

### Web3

A middleware between frontend and Ethereum blockchain. Basically it helps us calling contract methods (written in `Solidity`) with `javascript` directly

- Note that when you use this `send` **(Not `call`)** function, you get a receipt instead of the return value
- The method `web3.eth.send[Signed]Transaction` can't return named events because the method doesn't know about the contract ABI. With the unlocking of an account in the local Web3.js wallet will every transaction signed locally when you send a transaction.
