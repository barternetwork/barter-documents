---
sidebar_position: 7
---
# Butter桥
Butter桥允许将支持的代币从一个区块链桥接到另一个区块链。


## 请求参数 
```typescript
export type BridgeRequestParam = {
  fromAddress: string; // from address
  fromToken: BaseCurrency; // token to be bridged
  fromChainId: string; // from chain id
  toChainId: string; // to chain id
  toAddress: string; // destination chain receiving address
  amount: string; // amount to bridge in minimal uint
  options: ButterTransactionOption; // options
};
```
其中`ButterTransactionOption`包含完成桥接交易的所有必要信息。

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
`signerOrProvider`。Butter同时支持_ethers.js_和_web3.js_。如果您使用ethers.js，请提供[`Signer`](https://docs.ethers.org/v5/api/signer/)对象。如果您的应用选择使用web3.js，请提供[`Eth`](https://web3js.readthedocs.io/en/v1.2.11/web3-eth.html)对象，以便发送交易。

`nearProvider`: 当从Near协议发送交易时，您必须提供[`NearNetworkConfig`](https://near.github.io/near-api-js/interfaces/connect.ConnectConfig)，并提供keystore或[`WalletConnection`](https://near.github.io/near-api-js/classes/walletAccount.WalletConnection/)对象。

## 响应类型
请参考 [response type](types#buttertransactionresponse).

## 燃料费预估
预估Bridge交易的燃料费
```typescript
async function gasEstimateBridgeToken({
    fromAddress,
    fromToken,
    fromChainId,
    toChainId,
    toAddress,
    amount,
    options,
}: BridgeRequestParam): Promise<string>; // estimated gas in string
```
## 执行
```typescript
async function bridgeToken({
    fromAddress,
    fromToken,
    fromChainId,
    toChainId,
    toAddress, // recipient address
    amount, // amount of 'fromToken' to bridge
    options
}: BridgeRequestParam): Promise<ButterTransactionResponse> {};
```

##### 例1: 使用web3.js将1个USDC从以太坊主网桥接到BNB Chain

```typescript
// initiate ButterBridge Class
import {ButterTransactionResponse} from "./responseTypes";
import {PromiEvent} from "web3-core";

// create a Butter bridge instance.
const bridge: ButterBridge = new ButterBridge();

// assemble bridge request parameters
const bridgeRequest: BridgeRequestParam = {
    fromToken: ETH_MAINNET_USDC,
    fromChainId: ChainId.ETH_MAINNET,
    toChainId: ChainId.BSC_MAINNET,
    toAddress: '0x...',
    amount: '1000000',
    options: {
        signerOrProvider: web3.eth, // here we use web3.js as example
    },
};

const response: ButterTransactionResponse = await bridge.bridgeToken(
    bridgeRequest
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

##### 例2：使用ethers.js从以太坊主网桥接1个USDC到BSC

```typescript
// initiate ButterBridge Class
import {ButterTransactionReceipt, ButterTransactionResponse} from "./responseTypes";
import {PromiEvent} from "web3-core";

const bridge: ButterBridge = new ButterBridge();

// assemble bridge request parameters
const bridgeRequest: BridgeRequestParam = {
    fromToken: ETH_MAINNET_USDC,
    fromChainId: ChainId.ETH_MAINNET,
    toChainId: ChainId.BSC_MAINNET,
    toAddress: '0x...',
    amount: '1000000',
    options: {
        signerOrProvider: ethers.signer, // here we use ethers.js as example
    },
};

const response: ButterTransactionResponse = await bridge.bridgeToken(
    bridgeRequest
);
console.log("transaction hash", response.hash!)

const receipt: ButterTransactionReceipt = await response.wait!();
console.log('receipt', receipt)
```

##### 输出:
```text
transaction hash 0x..... 
receipt {
  to: string;
  from: string;
  gasUsed: string;
  transactionHash: string;
  blockHash?: string;
  blockNumber?: number;
  success?: boolean; // 1 success, 0 failed
}
```
