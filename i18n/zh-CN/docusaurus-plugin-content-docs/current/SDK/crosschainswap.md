---
sidebar_position: 9
---
# 跨链兑换
Butter跨链swap允许您从任何区块链上兑换任何代币。

## 请求
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
`swapRouteStr`：基于代币和提供的金额的最佳跨链互换路线。请参考[如何获得最佳路线](route#获取最佳路径)

`ButterTransactionOption`: 包含完成交易的所有必要信息。

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
`signerOrProvider`。Butter同时支持_ethers.js_和_web3.js_。如果您使用ethers.js，请提供[`Signer`](https://docs.ethers.org/v5/api/signer/)对象。如果您的应用程序选择使用web3.js，请提供[`Eth`](https://web3js.readthedocs.io/en/v1.2.11/web3-eth.html)对象，以便发送交易。

`nearProvider`: 当从Near协议发送交易时，您必须提供[`NearNetworkConfig`](https://near.github.io/near-api-js/interfaces/connect.ConnectConfig)，并提供keystore或[`WalletConnection`](https://near.github.io/near-api-js/classes/walletAccount.WalletConnection/)对象。

## 请求返回
请参考[response type](types#buttertransactionresponse).

## 燃料费预估
预估swap交易的燃料费

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
## 执行Swap
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
#### 示例1：将1个BNB换成Matic（web3.js）
```typescript
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
##### 输出:
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

#### 例2: 将1个BNB换成Matic（ethers.js）

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
##### 输出:
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
