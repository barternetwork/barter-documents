---
sidebar_position: 6
---
# 金库
在每条Butter支持的区块链上，我们都会部署一个"金库"合约，用来持有一定数量的代币流动性用于Bridge/Swap。

## 金库余额
使用下面方法获取金库余额
```typescript
const mapRpcProvider = {
    url: 'https://poc2-rpc.maplabs.io',
    chainId: 22776,
}

async function getVaultBalance(
  fromChainId: string, // from chain id
  fromToken: BaseCurrency, // from token
  toChainId: string, // to chain id
  mapRpcProvider: ButterJsonRpcProvider // map relay chain rpc provider
): Promise<VaultBalance>;
```

##### 示例：获得Butter金库中，有多少USDC可以从以太坊转到BNB Chain
```typescript
  const balance: VaultBalance = await getVaultBalance(
    ChainId.ETH_MAINNET,
    ETH_MAINNET_USDC,
    ChainId.BSC_MAINNET,
    mapProvider
  );
  console.log('vault balance', balance);
```

##### 输出:
```
vault balance {
  feeToken: Token {
    address: '0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d',
    chainId: '56',
    decimals: 18,
    symbol: 'USDC',
    name: 'Binance-Peg USD Coin',
    logo: 'https://files.maplabs.io/bridge/usdc.png',
    isNative: false,
    isToken: true
  },
  balance: '100000000000000000000',
  isMintable: false
}
````

## 响应
```typescript
interface VaultBalance {
  token: BaseCurrency; // token in vault
  balance: string; // amount of token in vault on target chain
  isMintable: boolean; // if token is mintable by Butter
}
```