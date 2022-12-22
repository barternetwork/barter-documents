# Types
This section includes the detail of all the request&response types needed while using the SDK.

## Request Types
All the request types needed to interact with the SDK
### `BridgeRequestParam`
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
The parameters needed for cross-chain swap action
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

### `ButterTransactionOption`
options needed to do a cross-chain swap
```typescript
export type ButterTransactionOption = {
  signerOrProvider?: Signer | Provider | Eth; // When source chain is EVM provide Ethers.js Signer/Provider or Web3.js Eth info
  nearProvider?: NearProviderType; // mandatory when src chain is near
  gas?: string;
  gasPrice?: string;
};
```
Whenever send a transaction from Near Protocol, you have to provide `NearNetworkConfig` with keystore provided or `WalletConnection` object
```typescript
export type NearProviderType = NearNetworkConfig | WalletConnection;
```

## Response Types
All the response type needed while using the SDK

### `ButterTransactionResponse`
Response datatype when invoke cross-chain bridge/swap

```typescript
export interface ButterTransactionResponse {
  hash: string;
  wait?: () => Promise<ButterTransactionReceipt>;
  promiReceipt?: PromiEvent<Web3TransactionReceipt>; // only when use web3.js
}
```
`promiReceipt` is only available when you are using web.js.

### `ButterTransactionReceipt`
Transaction receipt
```typescript
export interface ButterTransactionReceipt {
  to: string;
  from: string;
  gasUsed: string;
  transactionHash: string;
  logs: Array<Log> | string[]; // string[] for near logs
  blockHash?: string;
  blockNumber?: number;
  success?: boolean; // 1 success, 0 failed
}
```
