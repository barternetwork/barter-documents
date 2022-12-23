---
sidebar_position: 9
---
# Cross-Chain Swap
Butter cross-chain swap allows you to swap any token from any blockchain.

## Request
```typescript
export type SwapRequestParam = {
    fromAddress: string;
    fromToken: BaseCurrency;
    toAddress: string;
    toToken: BaseCurrency;
    amountIn: string; // in minimal uint
    swapRouteStr: string; // cross-chain swap route, in string format.
    slippage?: number; // in bps, e.g. 100 = 1%
    options: ButterTransactionOption;
};
```
`swapRouteStr`: optimal cross-chain swap route based on token and amount provided. Please see how to [get the best route](routes#get-the-best-route)

`ButterTransactionOption` contains all the necessary information required to complete a transaction:

```typescript
export type ButterTransactionOption = {
  signerOrProvider?: Signer | Provider | Eth; // When source chain is EVM provide Ethers.js Signer/Provider or Web3.js Eth info
  nearProvider?: NearProviderType; // mandatory when src chain is near
  gas?: string;
  gasPrice?: string;
};

// when send transaction from Near Protocol
type NearProviderType = NearNetworkConfig | WalletConnection;
```
`signerOrProvider`: Butter supports both _ethers.js_ and _web3.js_. If you are using ethers.js, provider the [`Signer`](https://docs.ethers.org/v5/api/signer/) object. If your application choose to use web3.js, please provide [`Eth`](https://web3js.readthedocs.io/en/v1.2.11/web3-eth.html) object in order to send a transaction.

`nearProvider`: Whenever send a transaction from Near Protocol, you have to provide [`NearNetworkConfig`](https://near.github.io/near-api-js/interfaces/connect.ConnectConfig) with keystore provided or [`WalletConnection`](https://near.github.io/near-api-js/classes/walletAccount.WalletConnection/) object

## Response
Please refer to [response type](types#buttertransactionresponse).

## Gas estimation
Estimate the gas cost of swap transaction
```typescript
async function gasEstimateSwap({
    fromAddress,
    fromToken,
    toAddress,
    toToken,
    amountIn,
    swapRouteStr,
    slippage,
    options
}: SwapRequestParam): Promise<string>; // estimated gas in string
```
## Execute Swap
```typescript
async function omnichainSwap({
    fromAddress,
    fromToken,
    toAddress,
    toToken,
    amountIn, // amount of 'fromToken' to swap
    swapRouteStr,
    slippage, // in bps
    options
}: SwapRequestParam): Promise<ButterTransactionResponse>;
 ```
#### Example 1: Swap 1 BNB for Matic using web3.js

```typescript
// initiate ButterSwap Class
import {ButterTransactionResponse} from "./responseTypes";
import {PromiEvent} from "web3-core";

// create a Butter swap instance.
const butterSwap: ButterSwap = new ButterSwap();

// assemble swap request parameters
const swapRequest: SwapRequestParam = {
    fromAddress: '0x...',
    fromToken: BNB_NATIVE,
    toAddress: '0x...',
    toToken: MATIC_NATIVE,
    amountIn: ethers.utils.parseEther('1'),
    swapRouteStr: '{}', // too long will omit here for readability
    slippage: 100, // 1% splippage
    options: {
        signerOrProvider: web3.eth, // here we use web3.js as example
    }
};

const response: ButterTransactionResponse = await butterSwap.omnichainSwap(
    swapRequest
);

const promiReceipt: PromiEvent<TransactionReceipt> = response.promiReceipt!;

await promiReceipt
    .on('transactionHash', function (hash: string) {
        console.log('hash', hash);
    })
    .on('receipt', function (receipt: any) {
        console.log('receipt', receipt);
    });

```
##### Output:
```
hash 0x..... // transaction hash
receipt { // web3.js TransactionReceipt
    status: boolean;
    transactionHash: string;
    transactionIndex: number;
    blockHash: string;
    blockNumber: number;
    from: string;
    to: string;
    contractAddress?: string;
    cumulativeGasUsed: number;
    gasUsed: number;
    effectiveGasPrice: number;
    logs: Log[]..;
    logsBloom: string;
    events?: {
        [eventName: string]: EventLog;
    };
}
```

#### Example 2: Swap 1 BNB for Matic using ethers.js

```typescript
import {ButterTransactionReceipt, ButterTransactionResponse} from "./responseTypes";
import {PromiEvent} from "web3-core";

const butterSwap: ButterSwap = new ButterSwap();

// assemble swap request parameters
const swapRequest: SwapRequestParam = {
    fromAddress: '0x...',
    fromToken: BNB_NATIVE,
    toAddress: '0x...',
    toToken: MATIC_NATIVE,
    amountIn: ethers.utils.parseEther('1'),
    swapRouteStr: '{}', // too long will omit here for readability
    slippage: 150, // 1.5% splippage
    options: {
        signerOrProvider: ethers.signer, // here we use web3.js as example
    },
};

const response: ButterTransactionResponse = await butterSwap.omnichainSwap(
    swapRequest
);
console.log("transaction hash", response.hash!)

const receipt: ButterTransactionReceipt = await response.wait!();
console.log('receipt', receipt)

```
##### Output:
```
transaction hash 0x..... 
receipt {
  to: string;
  from: string;
  gasUsed: string;
  transactionHash: string;
  blockHash?: string;
  logs?: Log[];
  blockNumber?: number;
  success?: boolean; // 1 success, 0 failed
}
```
