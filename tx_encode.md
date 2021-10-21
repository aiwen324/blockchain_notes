## Transaction Encoding and Decoding
Here is an input of Alice transaction. **Notice**: 1 byte = 2 hex digits


Consider the following raw transaction:
`0100000001186f9f998a5aa6f048e51dd8419a14d8a0f1a8a2836dd734d2804fe65fa35779000000008b483045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1deccbb6498c75c4ae24cb02204b9f039ff08df09cbe9f6addac960298cad530a863ea8f53982c09db8f6e381301410484ecc0d46f1918b30928fa0e4ed99f16a0fb4fde0735e7ade8416ab9fe423cc5412336376789d172787ec3457eee41c04f4938de5cc17b4a10fa336a8d752adfffffffff0260e31600000000001976a914ab68025513c3dbd2f7b92a94e0581f5d50f654e788acd0ef8000000000001976a9147f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a888ac00000000`


Now let's add spaces to make it clear, also see the notation below

`01000000 01 186f9f998a5aa6f048e51dd8419a14d8a0f1a8a2836dd734d2804fe65fa35779 00000000 8b 48 3045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1deccbb6498c75c4ae24cb02204b9f039ff08df09cbe9f6addac960298cad530a863ea8f53982c09db8f6e381301410484ecc0d46f1918b30928fa0e4ed99f16a0fb4fde0735e7ade8416ab9fe423cc5412336376789d172787ec3457eee41c04f4938de5cc17b4a10fa336a8d752adf ffffffff 02 60e3160000000000 19 76 a9 14 ab68025513c3dbd2f7b92a94e0581f5d50f654e7 88 ac d0ef800000000000 19 76 a9 14 7f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a8 88 ac 00000000`

Following is the annotation
```text
01000000 ..................................... version
01 ........................................... number of inputs
|
| 186f9f998a5aa6f048e51dd8419a14d8
  a0f1a8a2836dd734d2804fe65fa35779 ........... outpoint TXID
| 00000000 ................................... outpoint index number
| 8b ......................................... bytes in sig script: 139
| | 48 ....................................... push 72 bytes as data
| | | 3045022100884d142d86652a3f47ba4746ec719bbfbd04
| | | 0a570b1deccbb6498c75c4ae24cb02204b9f039ff08df0
| | | 9cbe9f6addac960298cad530a863ea8f53982c09db8f6e
| | | 381301410484ecc0d46f1918b30928fa0e4ed99f16a0fb
| | | 4fde0735e7ade8416ab9fe423cc5412336376789d17278
| | | 7ec3457eee41c04f4938de5cc17b4a10fa336a8d752adf ... Secp256k1 Signature
|
| ffffffff sequnce number (unit32)
| 

02 ........................................... number of outputs
| 60e3160000000000 ........................... Satoshis (8 bytes, little-endian order)
| 19 ......................................... bytes in pubkey script: 25
| | 76 ....................................... OP_DUP
| | a9 ....................................... OP_HASH160
| | 14 ....................................... Push 20 bytes as data
| | | ab68025513c3dbd2f7b92a94e0581f5d50f654e7 PubKey hash
| | 80 ....................................... OP_EQUALVERIFY
| | ac ....................................... OP_CHECKSIG
| d0ef800000000000 ........................... Satoshis (2nd output amount, 80efd0 = 8450000)
| 19 ......................................... bytes in pubkey script: 25
| | 76 ....................................... OP_DUP
| | a9 ....................................... OP_HASH160
| | 14 ....................................... Push 20 bytes as data
| | | 7f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a8 PubKey hash
| | 80 ....................................... OP_EQUALVERIFY
| | ac ....................................... OP_CHECKSIG
00000000 ..................................... lock time (4 bytes)
```



### Transaction Script Type 
Notice this might be how the json output know the transaction script type
```
| 19 ......................................... bytes in pubkey script: 25
| | 76 ....................................... OP_DUP
| | a9 ....................................... OP_HASH160
| | 14 ....................................... Push 20 bytes as data
| | | ab68025513c3dbd2f7b92a94e0581f5d50f654e7 PubKey hash
| | 80 ....................................... OP_EQUALVERIFY
| | ac ....................................... OP_CHECKSIG
```
Different transaction script types have different script templates. That's how we could determine the transaction script types.


Here is the json output from https://live.blockcypher.com/btc/decodetx/

```json
{
    "addresses": [
        "1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK",
        "1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA"
    ],
    "block_height": -1,
    "block_index": -1,
    "confirmations": 0,
    "double_spend": false,
    "fees": 50000,
    "hash": "0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2",
    "inputs": [
        {
            "addresses": [
                "1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK"
            ],
            "age": 277298,
            "output_index": 0,
            "output_value": 10000000,
            "prev_hash": "7957a35fe64f80d234d76d83a2a8f1a0d8149a41d81de548f0a65a8a999f6f18",
            "script": "483045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1deccbb6498c75c4ae24cb02204b9f039ff08df09cbe9f6addac960298cad530a863ea8f53982c09db8f6e381301410484ecc0d46f1918b30928fa0e4ed99f16a0fb4fde0735e7ade8416ab9fe423cc5412336376789d172787ec3457eee41c04f4938de5cc17b4a10fa336a8d752adf",
            "script_type": "pay-to-pubkey-hash",
            "sequence": 4294967295
        }
    ],
    "outputs": [
        {
            "addresses": [
                "1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA"
            ],
            "script": "76a914ab68025513c3dbd2f7b92a94e0581f5d50f654e788ac",
            "script_type": "pay-to-pubkey-hash",
            "value": 1500000
        },
        {
            "addresses": [
                "1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK"
            ],
            "script": "76a9147f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a888ac",
            "script_type": "pay-to-pubkey-hash",
            "value": 8450000
        }
    ],
    "preference": "high",
    "received": "2021-10-21T00:46:34.425685052Z",
    "relayed_by": "3.80.98.240",
    "size": 258,
    "total": 9950000,
    "ver": 1,
    "vin_sz": 1,
    "vout_sz": 2,
    "vsize": 258
}
```


### Signature
In this example, signature is 72 bytes
```
3045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1deccbb6498c75c4ae24cb02204b9f039ff08df09cbe9f6addac960298cad530a863ea8f53982c09db8f6e381301
```

```
30 # DER sequence tag
  45 # sequence length 0x45 (69) bytes
    02 # integer element
      21 # element length 0x21 (33) bytes
        00884d142d86652a3f47ba4746ec719bbfbd040a570b1deccbb6498c75c4ae24cb # ECDSA r value
    02 # integer element
      20 # element length 0x20 (32) bytes
        4b9f039ff08df09cbe9f6addac960298cad530a863ea8f53982c09db8f6e3813 # ECDSA s value

01 # DER encoding completed
```
That one byte left over after the DER encoding (but still part of the actual signature push) is the SIGHASH flag. The following is the definition of different SIGHASH flag
```c
/** Signature hash types/flags */
enum
{
    SIGHASH_ALL = 1,
    SIGHASH_NONE = 2,
    SIGHASH_SINGLE = 3,
    SIGHASH_ANYONECANPAY = 0x80,
};
```

### Public Key Extraction
Public key is right after the signature. 
`410484ecc0d46f1918b30928fa0e4ed99f16a0fb4fde0735e7ade8416ab9fe423cc5412336376789d172787ec3457eee41c04f4938de5cc17b4a10fa336a8d752adf`

```
41 # public key length 0x41 (65) bytes (Uncompressed, compressed key is 33 bytes)
  0484ecc0d46f1918b30928fa0e4ed99f16a0fb4fde0735e7ade8416ab9fe423cc5412336376789d172787ec3457eee41c04f4938de5cc17b4a10fa336a8d752adf # public key string
```
