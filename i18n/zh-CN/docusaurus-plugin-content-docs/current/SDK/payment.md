---
sidebar_position: 10
---
# 全链支付(Beta)
Butter全链支付功能允许用户支付任何加密货币，无论商家需要哪条链上的那种加密货币

## 求情
```typescript
export type PaymentRequestParam = {
    fromAddress: string;
    paidToken: BaseCurrency; // 用户支付的代币
    paidAmount: string; // 支付的代币数量
    toAddress: string; // 收款地址 
    requiredToken: BaseCurrency; // 收款代币
    requiredAmount: string; // 应付款
    swapRouteStr: string;
    options: ButterTransactionOption;
};
```
`paidAmount`: 用户应支付的代币数量。请参考[如何获取应支付代币数量](#paidamount)

`swapRouteStr`：基于代币和提供的金额的最佳跨链互换路线。请参考[如何获得最佳路线](routes#bestroute)

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

## 获取应付代币数量 <a name="paidamount"></a>
首先，我们需要确定为了达到`所需金额`而需要的`支付代币`的数量。

例如，爱丽丝想在以太坊上购买一个价格为400 $USDC的物品，但她想用BNB主币支付。所以应该有一种方法来确定她需要支付多少$BNB代币给卖家以满足以太坊上所需的400 $USDC。

Butter提供了`getEstimateAmountInFromRequiredAmount()`方法。

```typescript
export async function getEstimateAmountInFromRequiredAmount(
  fromToken: BaseCurrency, // paid token, in the above case BNB native coin.
  toToken: BaseCurrency, // required token, in the above case ETH_USDC.
  requiredAmount: string // required amount, in the above case 400 $USDC.
): Promise<string>;
```
返回值就是所需的`fromToken'的预估数量。

## 燃料费预估
预估支付交易的燃料费
```typescript
async function gasEstimateOmniPay({
    fromAddress,
    paidToken,
    paidAmount, // in minimal uint
    toAddress,
    requiredToken,
    requiredAmount,
    swapRouteStr,
    options,
}: PaymentRequestParam): Promise<string>
```

## 执行支付
```typescript
async function omniPay({
    fromAddress,
    paidToken,
    paidAmount, // in minimal uint
    toAddress,
    requiredToken,
    requiredAmount,
    swapRouteStr,
    options,
}: PaymentRequestParam): Promise<ButterTransactionResponse>;
 ```
#### Example 1: 爱丽丝想用BNB原生币支付以太坊上一个需要支付$USDC的物品，价格为400 $USDC。

```typescript
import {ButterTransactionResponse} from "./responseTypes";
import {PromiEvent} from "web3-core";

// 首先我们需要得到需要多少$BNB原生币。
const paidAmount = await getEstimateAmountInFromRequiredAmount(
    BNB_NATIVE,
    ETH_USDC,
    '400000000'
)

// 创建一个全链支付实例。
const omniPayment: OmniPayment = new OmniPayment();

// 拼装支付交易请求
const paymentRequest: PaymentRequestParam = {
    fromAddress: '0x...',
    paidToken: BNB_NATIVE,
    paidAmount: paidAmount, // in minimal uint
    toAddress: '0x...',
    requiredToken: ETH_USDC,
    requiredAmount: 400000000,
    options: {
        signerOrProvider: web3.eth, // here we use web3.js as example
    }
};

const response: ButterTransactionResponse = await omniPayment.omniPay(
    paymentRequest
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
