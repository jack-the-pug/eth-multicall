# Multicall.js

Utilising the Ethereum Multi-call contract, this package helps engages with it by automatically flattening, encoding, decoding and un-flattening.

## Install

    yarn add multicall.js

### Multicall Contract Addresses

| Chain   | Address |
| ------- | ------- |
| Mainnet | [0xeefba1e63905ef1d7acba5a8513c70307c1ce441](https://etherscan.io/address/0xeefba1e63905ef1d7acba5a8513c70307c1ce441#contracts) |
| BSC | [0xB94858b0bB5437498F5453A16039337e5Fdc269C](https://bscscan.com/address/0xB94858b0bB5437498F5453A16039337e5Fdc269C#contracts) |
| Polygon | [0xa0ea094f5d9495f1e4bbe78799d27a34206e4e72](https://polygonscan.com/address/0xa0ea094f5d9495f1e4bbe78799d27a34206e4e72)


## Example

Constructor params

- Web3 instance
- (Defaults to main net) Multicall contract address
- (Defaults to [300, 100, 25]) Chunk sizes

```
import { MultiCall } from 'eth-multicall'
const web3 = new Web3(...)
const multiCallContract = '0x5Eb3fa2DFECdDe21C950813C665E9364fa609bD2'
const multicall = new MultiCall(web3, multiCallContract);
```

## Request

```

 const addresses = [
   "0x960b236A07cf122663c4303350609A66A7B288C0",
   "0x1f573d6fb3f13d689ff844b4ce37794d79a7ff1c",
 ];

 const tokens = addresses.map((address) => {
   const token = new web3.eth.Contract(ABIERC20Token, address);
   return {
     symbol: token.methods.symbol(),
     decimals: token.methods.decimals(),
   };
 });

const [tokensRes] = await multiCall.all([tokens]);

[
    {
        symbol: "ANT",
        decimals: "18",
    },
    {
        symbol: "BNT",
        decimals: "18",
    }
]

```

- To add the contracts address to the response add a property with the string value 'originAddress' to the schema.

```


{
  symbol: token.methods.symbol(),
  decimals: token.methods.decimals(),
  tokenAddress: 'originAddress'
}

=>

{
    symbol: "ANT",
    decimals: "18",
    tokenAddress: "0x960b236A07cf122663c4303350609A66A7B288C0"
}

```

- Meta strings are also supported to track any data that you might wish to be retained

```


{
  symbol: token.methods.symbol(),
  decimals: token.methods.decimals(),
  tokenAddress: 'originAddress'
  foo: 'bar'
}

=>

{
    symbol: "ANT",
    decimals: "18",
    tokenAddress: "0x960b236A07cf122663c4303350609A66A7B288C0",
    foo: 'bar'
}

```

- Failed requests currently returning `undefined` with no sign or indication of error.
