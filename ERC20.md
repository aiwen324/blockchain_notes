- [Interfaces:](#interfaces)
  - [transfer vs transferFrom:](#transfer-vs-transferfrom)
- [Other Details](#other-details)
  - [Supply:](#supply)

## Interfaces:

### transfer vs transferFrom:

- `transfer` is used to do normal transaction, i.e. `A` transfer n amount of tokens to `B`.
- `transferFrom` can be used in the following scenario:

  `A` use `approve` to grant some `contract` (i.e. Faucet) certain amount of tokens (which is stored in allowed, a map from address to a map from address to ammount).

  ```json
  {'A':
    {
    'Faucet_address': n
    }
  }
  ```

  Then when `B` requests money from the `contract`, the contract can call `transferFrom(A, B, amount)` to transfer the amount of tokens from `A` to `B` without the necesarrity of `A` involved. Surely `transferFrom` need to take care of the amount addition/substraction in `allowed` and `balances`.

## Other Details
### Supply:

1. For `totalSupply`, it is not the amount of token, it equals to `amount_of_token * (10^decimals)`. If I want 1 token with the ability to subdivide it with a precision of 2 decimal places, I represent that as 100. That's why the ammount of token shown in `MetaMask` does not equal to the amount return in the console.
