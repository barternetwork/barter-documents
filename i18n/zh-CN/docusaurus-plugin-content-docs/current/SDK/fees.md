---
sidebar_position: 5
---
# 费用
Butter Bridge/Swap会收取少量费用，该费用主要用于预付目的链的燃料费。

## 响应类型
### `ButterFee`

```typescript
export interface ButterFee {
  feeToken: BaseCurrency; // the token that to be charged
  amount: string; // in minimal unit
  feeRate: ButterFeeRate; // fee rate inforamtion
  feeDistribution?: ButterFeeDistribution; // fee distribution inforamtion
}
```

### `ButterFeeRate`
费率
```typescript
export type ButterFeeRate = {
  lowest: string; // lowest AMOUNT of fee token to be charged
  highest: string; // highest AMOUNT of fee token to be charged
  rate: string; // fee rate in bps
};
```

### `ButterFeeDistribution`
费率分配对象
```typescript
export type ButterFeeDistribution = {
  protocol: string; // protocol rate in bps
  relayer: string; // relayer rate in bps, prepaid destination gas fee
  lp: string; // lp fee rate in bps
};
```

## Bridge费率
请使用`getBridgeFee`方法获取Bridge费率
```typescript
/**
 * get fee for bridging srcToken to targetChain
 * @param srcToken: source token to bridge
 * @param targetChain: target chain
 * @param amount: bridge amount in minimal uint
 * @param mapRpcProvider: map relay chain rpc provider
 */
async function getBridgeFee(
  srcToken: BaseCurrency,
  targetChain: string,
  amount: string,
  mapRpcProvider: ButterJsonRpcProvider
): Promise<ButterFee>

// rpc provider format
type ButterJsonRpcProvider = {
    chainId: number;
    // note here should provide the RPC URL for MAP Relay Chain,
    // since all the fee info is stored on MAP Relay Chain
    url?: string; // use default if not presented, 
};
```

##### 示例: 获得从BNB Chain Bridge 1个USDC到的费用。
```typescript
// MAP Relay Chain的RPC节点信息
const mapRpcProvider = {
    url: 'https://poc2-rpc.maplabs.io', 
    chainId: '22776',
}

// get the fees for bridging one ether from Ethereum Mainnet to Binance Smart Chain
const fee: ButterFee = await getBridgeFee(
    BSC_USDC, // srcToken
    ChainId.POLYGON_MAINNET, // targetChain
    ethers.utils.parseUints('1', BSC_USDC.decimals), // amount
    mapRpcProvider
)

console.log('bridge fee', fee);
```
##### 输出
```json
bridge fee {
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
  feeRate: {
    lowest: '200000000000000000', // lowest amount of feeToken to charge
    rate: '10', // fee rate in bps, here would be 0.1%
    highest: '1000000000000000000' // highest amount of feeToken to charge
  },
  amount: '200000000000000000', // fee amount in feeToken
  feeDistribution: { relayer: '9000', lp: '1000', protocol: '0' }

}

// 这意味着将USDC从BSC桥接到Ethereum要收取0.1%的费用，最低0.2usdc，最高1usdc。
// 在这种情况下，1个USDC的桥接费用为0.2个USDC，扣除费用后用户将得到0.8个USDC。

```
## Swap费率
请使用`getSwapFee`方法来获取Swap费率
```typescript
/**
 * get fee for cross-chain exchange
 * @param srcToken source token
 * @param targetChain target chain id
 * @param amount amount in minimal uint
 * @param routeStr cross-chain route in string format
 * @param mapRpcProvider map relay chain rpc provider
 */
export async function getSwapFee(
  srcToken: BaseCurrency,
  targetChain: string,
  amount: string,
  routeStr: string,
  mapRpcProvider: ButterJsonRpcProvider
): Promise<ButterFee>

type ButterJsonRpcProvider = {
    chainId: number;
    // note here should provide the RPC URL for MAP Relay Chain,
    // since all the fee info is stored on MAP Relay Chain
    url?: number; // use default if not presented, 
};
```
##### 示例: 获得从BNB链主网交换1个BNB到Polygon主网的Matic的费用。

```typescript
const mapRpcProvider = {
    url: 'https://poc2-rpc.maplabs.io', 
    chainId: 22776,
}

// get the fees for bridging one ether from Ethereum Mainnet to Binance Smart Chain
const fee: ButterFee = await getSwapFee(
    BSC_BNB, // srcToken
    ChainId.POLYGON_MAINNET, // targetChain
    ethers.utils.parseUints('1', BSC_USDC.decimals), // amount
    '{}', // here is the swap route you get.
    mapRpcProvider
)

console.log('bridge fee', fee);
```
##### 输出
```json
swap fee {
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
  feeRate: {
    lowest: '200000000000000000', // lowest amount of feeToken to charge, 0.2 usdc
    rate: '10', // fee rate in bps, here would be 0.1%
    highest: '1000000000000000000' // highest amount of feeToken to charge. 1 usdc
  },
  amount: '300000000000000000', // fee amount in feeToken
  feeDistribution: { relayer: '9000', lp: '1000', protocol: '0' }
}
```
假设1个BNB=300美元，上述费用说明Butter将收取300美元的0.1%的费用，也就是0.3美元，其中90%归中继者用于目标链gas补贴，10%归流动性提供者。

