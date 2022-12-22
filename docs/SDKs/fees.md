_# Fees
Butter charges a small fee for bridging and swapping, the fee is mainly used to pre-pay the destination gas fees.
## Response Types
### `ButterFee`
Fee object
```typescript
export interface ButterFee {
  feeToken: BaseCurrency; // the token that to be charged
  amount: string; // in minimal unit
  feeRate: ButterFeeRate; // fee rate inforamtion
  feeDistribution?: ButterFeeDistribution; // fee distribution inforamtion
}
```

### `ButterFeeRate`
Fee rate
```typescript
export type ButterFeeRate = {
  lowest: string; // lowest AMOUNT of fee token to be charged
  highest: string; // highest AMOUNT of fee token to be charged
  rate: string; // fee rate in bps
};
```

### `ButterFeeDistribution`
Fee distribution
```typescript
export type ButterFeeDistribution = {
  protocol: string; // protocol rate in bps
  relayer: string; // relayer rate in bps, prepaid destination gas fee
  lp: string; // lp fee rate in bps
};
```

## Bridge Fee
To get the bridge fee charged by Butter, use `getBridgeFee` method
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
##### Example: get the fee for bridging 1 USDC from BNB Chain Mainnet to Polygon Mainnet.
```typescript
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
##### Output
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

// This means bridging USDC from BSC to Ethereum charge a feee of 0.1%, with minimum 0.2 usdc and maximum 1 usdc.
// In this case bridging 1 usdc would have a fee of 0.2 usdc, user will get 0.8 usdc after the fee deduction.

```
## Swap Fee
To get the swap fee charged by Butter, use `getSwapFee` method
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
##### Example: get the fee for swapping 1 BNB from BNB Chain Mainnet for Matic on Polygon Mainnet.
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
##### Output
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
Assume 1 BNB = 300 USDC, the above fee states Butter charges a 0.1% fee of 300 USDC which is 0.3 usdc, of which of 90% goes to relayer and 10% goes to the liquidity provider.

