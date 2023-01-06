---
sidebar_position: 4
---
# 请求和响应
本节包括使用SDK时需要的所有请求和响应类型的细节。

## 请求类型

### `BridgeRequestParam`
Bridge的请求参数数据结构。

```typescript
export type BridgeRequestParam = {
  fromAddress: string;
  fromToken: BaseCurrency;
  fromChainId: string;
  toChainId: string;
  toAddress: string;
  amount: string; // in minimal uint
  options: ButterTransactionOption;
};
```

### `SwapRequestParam`
Swap的请求参数数据结构。
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
### `PaymentRequestParam`
支付请求参数
```typescript
export type PaymentRequestParam = {
    fromAddress: string;
    paidToken: BaseCurrency; // token user paid
    paidAmount: string; // in minimal uint
    toAddress: string; // seller's receiving address 
    requiredToken: BaseCurrency; // required token by seller
    requiredAmount: string; // required amount
    swapRouteStr: string;
    options: ButterTransactionOption;
};
```

### `ButterTransactionOption`
Butter交易的option参数
```typescript
export type ButterTransactionOption = {
  signerOrProvider?: Signer | Provider | Eth; // When source chain is EVM provide Ethers.js Signer/Provider or Web3.js Eth info
  nearProvider?: NearProviderType; // mandatory when src chain is near
  gas?: string;
  gasPrice?: string;
};
```
`signerOrProvider`。Butter同时支持_ethers.js_和_web3.js_。如果您使用ethers.js，请提供[`Signer`](https://docs.ethers.org/v5/api/signer/)对象。如果您的应用程序选择使用web3.js，请提供[`Eth`](https://web3js.readthedocs.io/en/v1.2.11/web3-eth.html)对象，以便发送交易。

`nearProvider`: 当从Near协议发送交易时，您必须提供[`NearNetworkConfig`](https://near.github.io/near-api-js/interfaces/connect.ConnectConfig)，并提供keystore或[`WalletConnection`](https://near.github.io/near-api-js/classes/walletAccount.WalletConnection/)对象。
```typescript
export type NearProviderType = NearNetworkConfig | WalletConnection;
```

## Response Types
### 金库
#### 金库余额
`VaultBalance`
```typescript
interface VaultBalance {
  token: BaseCurrency; // token in vault
  balance: string; // amount of token in vault on target chain
  isMintable: boolean; // if token is mintable by Butter
}
```

### 费用
#### `ButterFee`

```typescript
export interface ButterFee {
  feeToken: BaseCurrency; // the token that to be charged
  amount: string; // in minimal unit
  feeRate: ButterFeeRate; // fee rate inforamtion
  feeDistribution?: ButterFeeDistribution; // fee distribution inforamtion
}
```

#### 费率
`ButterFeeRate`
```typescript
export type ButterFeeRate = {
  lowest: string; // lowest AMOUNT of fee token to be charged
  highest: string; // highest AMOUNT of fee token to be charged
  rate: string; // fee rate in bps
};
```

#### 费率分布
`ButterFeeDistribution`
```typescript
export type ButterFeeDistribution = {
    protocol: string; // protocol rate in bps
    relayer: string; // relayer rate in bps, prepaid destination gas fee
    lp: string; // lp fee rate in bps
};
```

### 最佳路径
#### `RouteResponse`

```typescript
export type RouteResponse = {
  data?: string; // json string
  msg: string;
  status: number;
};
```

### Bridge/Swap交易返回
#### `ButterTransactionResponse`
```typescript
export interface ButterTransactionResponse {
  hash: string;
  wait?: () => Promise<ButterTransactionReceipt>;
  promiReceipt?: PromiEvent<Web3TransactionReceipt>; // only when use web3.js
}
```
[`promiReceipt`](https://web3js.readthedocs.io/en/v1.2.11/callbacks-promises-events.html)只有在您使用web.js时才可用。

#### `ButterTransactionReceipt`
```typescript
export interface ButterTransactionReceipt {
  to: string;
  from: string;
  gasUsed: string;
  transactionHash: string;
  logs: Array<Log> | string[]; // string[] for Near logs
  blockHash?: string;
  blockNumber?: number;
  success?: boolean; // 1 success, 0 failed
}
```
