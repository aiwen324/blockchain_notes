- [Basics](#basics)
- [Address](#address)
  - [Address Literals](#address-literals)
  - [`address` VS `address payable`](#address-vs-address-payable)
  - [Address Methods](#address-methods)
  - [The Zero Address](#the-zero-address)
- [Network ID and Chain ID](#network-id-and-chain-id)
- [MetaMask](#metamask)
  - [Tips](#tips)

## Basics

$1$ _Ether_ $= 1^9$ _gwei_ $= 1^{18}$ _wei_ $= 1,000,000,000,000,000,000$ _wei_

$1$ _gwei_ $= 1$ _shannon_ $= 1^9$ _wei_

The above _gwei_ and _shannon_ is the unit of gas price, not gas amount.

Let's say your **_gas limit_** is 6721975, and **_gas price_** is 100 shannon, this means the maximum amount of money (general meaning here) that you are willing to pay for this transaction is: $6721975 \times 100 = 672197500 = 6.72 * 10^8$ _shannon_ $= 0.672$ _Ether_

## Address

Ethereum uses the **keccak-256** hash function to generate them.

- Externally Owned Accounts (EOA)
- Contracts Accounts (Smart Contracts) : controlled by their code.

Here is an address example:

```js
address user = msg.sender
```

### Address Literals

Address literals are the hexadecimal representation of an Ethereum address, **hardcoded in a solidity file**.

An address literal must:

- contain 40 characters (20 bytes)
- be prefixed with `0x`
- have a valid checksum

### `address` VS `address payable`

- You can send ether to a variable defined as `address payable`
- You cannot send Ether to a variable defined as address
- Explicit conversion from and to `address` are allowed for : _integers_, _integer literals_, _bytes20_ and _contract_ type.

```js
contract Vaults {
    // United States gold reserves (cannot receive ether)
    address FortKnox;

    // deposit ether here
    address payable etherVault;
}
```

**Note**: The type returned by `msg.sender` is of `address payable` type.

```js
contract NotPayable {
     // No payable function here
}
contract Payable {
     // Payable function
     function() payable {
          // do something with payable function

     }
}
contract HelloWorld {
     address x = address(NotPayable);
     address y = address(Payable);
     function hello_world() public pure returns (string memory) {
          return "hello world";
     }
}
```

- `NotPayable` is a contract without a payable fallback function. The the variable `x` which is assigned the value `address(NotPayable)` will be of type `address`.
- `Payable` is a contract with a payable fallback function. The the variable `y` which is assigned the value `address(Payable)` will be of type `address payable`.

### Address Methods

**As a result, the following methods become available for an address defined as payable in Solidity: `.transfer()`, `.send()`, `.call()`, `.delegatecall()` and `.staticcall()`**

- methods related to ethers: `.balance()`, `.transfer()` and `.send()`
- methods related to contract interactions: `.call()`, `.delegatecall()` and `.staticcall()`

**Warning**: Avoid using `.send()` and `.call()`.

- There are some dangers in using `send`: The transfer fails if the call stack depth is at 1024 and it also fails if the recipient runs out of gas.
- Avoid using `.call()` whenveer possible when executing another contract function **as it bypassess type checking, function existence check, and argument packing**

### The Zero Address

The Ethereum Virtual Machine (EVM) understand that a transaction intends to create a new contract when it specifies a specific address known as **zero-address** in the recipient field.

The **zero-address** is a bit like a **_"fake account"_**. Like a normal Ethereum address, the **zero-address** is also 20 bytes long but contains only empty bytes. Hence it’s name **zero-address**, since it contains only 0x0 values, as shown below.

```
0x0000000000000000000000000000000000000000
```

## Network ID and Chain ID

Check this link: https://ethereum.stackexchange.com/questions/37533/what-is-a-chainid-in-ethereum-how-is-it-different-than-networkid-and-how-is-it/37536

ChainID was introduced in [EIP-155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md) to prevent replay attacks between the main ETH and ETC chains, which both have a networkID of 1.

It's basically just an additional way to tell chains apart. Subsequent to [EIP-155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md), ETH has a chainID of 1, while ETC has a chainID of 61 (even though they still have the same networkID of 1).

## MetaMask

### Tips

1. Avoid using the extension in a standalone window when you are trying to connect to some test websites.
