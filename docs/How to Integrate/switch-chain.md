# Butter Bridge

## Table of Contents
1. [Installation](#installation)
2. [Tokens and Chains](#tokenandchain)
3. [Fees](#fees)
4. [Vault Balance](#vaultbalance)
5. [Asset Bridging](#assetbridge)



<a name="installation"></a>
## Installation
```shell
# npm
npm i --save-dev butterjs-sdk

# yarn
yarn add butterjs-sdk
```
<a name="tokenandchain"></a>
## Tokens and Chains
Currently Butter only support limited chains and tokens and Butter will provide lists of supported chains and tokens in the format of constants.

```typescript
// To get supported blockchain id
const supportedChainIdList = SUPPORTED_CHAIN_LIST

// To get supported token list by chain id
const supportedTokenList = ID_TO_SUPPORTED_TOKEN('1')
// the above will list all the supported token with chainId = 1, 
// note here we use string as parameter type.
```

<a name="fees"></a>
## Fees
Butter charges a small fees for bridging or exchanging cross-chain assets. It is subject to what kind of token you want to bridge or swap.
<br>
### Bridging Fee
To get the bridging fee, use the following method:
```typescript
async function getBridgeFee(
    srcToken: BaseCurrency, // source token, can be the format of native coin or token
    targetChain: string, // target blockchain id
    amount: string, // amount to bridge, in minimal unit
    mapRpcProvider: ButterJsonRpcProvider // map relay chain rpc provider information
): Promise<ButterFee>

// Provider format
type ButterJsonRpcProvider = {
    chainId: number;
    // note here should provide the RPC URL for MAP Relay Chain,
    // since all the fee info is stored on MAP Relay Chain
    url?: string; // use default if not presented, 
};

// return type
interface ButterFee {
    feeToken: BaseCurrency; // fee currency to charge, usally in the format of source token
    amount: string; // amount to charge
    feeDistribution?: ButterFeeDistribution; // fee distribution, only swap has this field, bridge does not any distribution
}

type ButterFeeDistribution = {
    protocol: number; // base protocol fees in bps
    compensation: number; // gas compensation on target chain
    lp?: number;
};
```
##### Example: get the fee for bridging 1 Ether from Ehtereum Mainnet to BSC Mainnet.

```typescript
// MAP Relay Chain Mainnet Provider
const mapRpcProvider = {
    url: 'https://poc2-rpc.maplabs.io', 
    chainId: 22776,
}

// get the fees for bridging one ether from Ethereum Mainnet to Binance Smart Chain
const fee: ButterFee = await getBridgeFee(
    Ether, // srcToken
    ChainId.BSC_MAINNET, // targetChain
    '1000000000000000000', // amount
    mapRpcProvider
)

console.log("brige fee", fee);
``` 
##### Output

```
bridge fee {
    feeToken: Token {
        address: '0x0000000000000000000000000000000000000000',
            chainId: 1,
            decimals: 18,
            symbol: 'ETH',
            name: 'Ether',
            isNative: true,
            isToken: false
    },
    amount: '100000000000000',
}
// This represents bridging 1 ether, there is a 0.0001 ether deduct as bridging fee
// User will get 0.9999 ether eventually. 
```
<a name="vaultbalance"></a>
## Vault Balance
In Butter, we deploy one `Vault` smart contract for each blockchain we connected in order to hold asset on that chain. To get the balance of certain token in the vault
```typescript
async function getVaultBalance(
  fromChainId: string, // from chain id
  fromToken: BaseCurrency, // from token
  toChainId: string, // to chain id
  rpcProvider: ButterJsonRpcProvider // map relay chain rpc provider
): Promise<VaultBalance>;

// return type
interface VaultBalance {
  token: BaseCurrency; // vault token
  balance: string; // amount of token in target chain
}
```

##### Example: get the balance of USDC in the vault of BSC where source chain is Ethereum
```typescript
  const balance: VaultBalance = await getVaultBalance(
    ChainId.ETH_MAINNET,
    ETH_USDC,
    ChainId.BSC_MAINNET,
    provider
  );
  console.log('vault balance', balance);
```

##### Output:
```
vault balance {
  token: Token {
    // usdc address on BSC
    address: '0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d',
    // bsc chain Id
    chainId: '56', 
    decimals: 18,
    symbol: 'USDC',
    name: 'USD Circle',
    isNative: false,
    isToken: true
  },
  balance: '8000000000000000000000000'
}
// this represents map has 8000000 BSC-USDC available to transfer from ETH to BSC.

```
<a name="assetbridge"></a>
## Asset Bridging
Butter Bridge allows bridging supported tokens from one blockchain to another.<br>

### Gas estimation
```typescript
async function gasEstimateBridgeToken({
    fromAddress,
    token,
    toChainId,
    toAddress,
    amount,
    options,
}: BridgeRequestParam): Promise<string>; // estimated gas in string
```
<a name = "bridgeparam"></a>
### Parameters
```typescript
// BridgeRequestParam
type BridgeRequestParam = {
    fromAddress: string; // from account address
    token: BaseCurrency; // token to bridge
    fromChainId: string; // from chain id 
    toChainId: string; // to chain id
    toAddress: string; // to address
    amount: string; // amount to bridge
    options: BridgeOptions;
};

// BridgeOptions
type BridgeOptions = {
    // Provide Signer or Provider if you are using ethers.js
    // Provide Eth if you are using web3.js
    signerOrProvider?: Signer | Provider | Eth; // EVM chain signerOrProvider
    
    // Provide WalletConnection if you are connecting through frontend wallet
    // Provide NearNetworkConfig if you are connecting through KeyStore
    // Note Near does not support gas estimation yet.
    nearConfig?: NearNetworkConfig | WalletConnection; // when source chain is Near, provide nearConfig Object
    gas?: string; // maunally input gas
};

```
<a name = "txresult"></a>
### Transaction Return Type
```typescript
// ButterTransactionResponse
interface ButterTransactionResponse {
    hash?: string; // transaction hash
    wait?: () => Promise<ButterTransactionReceipt>; // wait function if using ethers.js or near
    promiReceipt?: PromiEvent<Web3TransactionReceipt>; // promiEvent when if web3.js
}

export interface ButterTransactionReceipt {
    to: string;
    from: string;
    gasUsed: string;
    transactionHash: string;
    blockHash?: string;
    blockNumber?: number;
    success?: boolean; // 1 success, 0 failed
}

```

### Bridge Token
```typescript
async function bridgeToken({
    fromAddress,
    fromToken,
    fromChainId,
    toChainId,
    toAddress, // recipient address
    amount, // amount of 'fromToken' to bridge
    options,
}: BridgeRequestParam): Promise<ButterTransactionResponse>;
 ```

for more detail on `BridgeRequestParam` and `ButterTransactionResponse`, please see [parameters](#bridgeparam) and [transaction return type](#txresult).

##### Example1: Bridge 1 USDC from Ethereum Mainnet to BSC using web3.js

```typescript
// initiate ButterBridge Class
import {ButterTransactionResponse} from "./responseTypes";
import {PromiEvent} from "web3-core";

const bridge: ButterBridge = new ButterBridge();

// assemble bridge request parameters
const bridgeRequest: BridgeRequestParam = {
    fromToken: ETH_BSC,
    fromChainId: ChainId.ETH_MAINNET,
    toChainId: ChainId.BSC_MAINNET,
    toAddress: '0x...',
    amount: '1000000000000000000',
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
    logs: Log[];
    logsBloom: string;
    events?: {
        [eventName: string]: EventLog;
    };
}
```

##### Example2: Bridge 1 USDC from Ethereum Mainnet to BSC using ethers.js

```typescript
// initiate ButterBridge Class
import {ButterTransactionReceipt, ButterTransactionResponse} from "./responseTypes";
import {PromiEvent} from "web3-core";

const bridge: ButterBridge = new ButterBridge();

// assemble bridge request parameters
const bridgeRequest: BridgeRequestParam = {
    fromToken: ETH_BSC,
    fromChainId: ChainId.ETH_MAINNET,
    toChainId: ChainId.BSC_MAINNET,
    toAddress: '0x...',
    amount: '1000000000000000000',
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
##### Output:
```
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
