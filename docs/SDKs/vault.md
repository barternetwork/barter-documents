# Vault Balance
On each connected blockchain, Butter has a 'Vault' that holds certain amount of token assets that is available to bridge/swap

## Return Type
```typescript
interface VaultBalance {
  token: BaseCurrency; // token in vault
  balance: string; // amount of token in vault on target chain
  isMintable: boolean; // if token is mintable by Butter
}
```

## Vault Balance
To get the vault balance, use the following provided method:
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

##### Example: get how many USDC tokens in our vault is available to transfer from Ethereum to BSC
```typescript
  const balance: VaultBalance = await getVaultBalance(
    ChainId.ETH_MAINNET,
    ETH_MAINNET_USDC,
    ChainId.BSC_MAINNET,
    mapProvider
  );
  console.log('vault balance', balance);
```

##### Output:
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
