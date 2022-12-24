---
sidebar_position: 2
---
# 实体
这里您可以了解所以SDK中定义的实体
## `BaseCurrency`
_BaseCurrency_是一个抽象的类，涵盖了所有类型的代币，包括原生币、标准代币和其他链上原生货币。

```typescript
export abstract class BaseCurrency {
  /**
   * Returns whether the currency is native to the chain and must be wrapped (e.g. EVMNativCoin)
   */
  public abstract readonly isNative: boolean;
  /**
   * Returns whether the currency is a token that is usable in Butter without wrapping
   */
  public abstract readonly isToken: boolean;

  /**
   * The chain ID on which this currency resides
   */
  public readonly chainId: string;
  /**
   * The decimals used in representing currency amounts
   */
  public readonly decimals: number;

  /**
   * The address of the token, 'ZERO_ADDRESS' when token is native token
   */
  public readonly address: string;
  /**
   * The symbol of the currency, i.e. a short textual non-unique identifier
   */
  public readonly symbol?: string;
  /**
   * The name of the currency, i.e. a descriptive textual non-unique identifier
   */
  public readonly name?: string;

  /**
   * logo of the token, for display only
   */
  public readonly logo?: string;
  /**
   * Constructs an instance of the base class `BaseCurrency`.
   * @param chainId the chain ID on which this currency resides
   * @param decimals decimals of the currency
   * @param address address of the currency
   * @param symbol symbol of the currency
   * @param name of the currency
   * @param logo of the currency
   */
  protected constructor(
    chainId: string,
    decimals: number,
    address: string,
    symbol?: string,
    name?: string,
    logo?: string
  ) {
    invariant(
      decimals >= 0 && decimals < 255 && Number.isInteger(decimals),
      'DECIMALS'
    );
    this.address = address;
    this.chainId = chainId;
    this.decimals = decimals;
    this.symbol = symbol;
    this.name = name;
    this.logo = logo;
  }

  /**
   * Returns whether this currency is functionally equivalent to the other currency
   * @param other the other currency
   */
  public abstract equals(other: Currency): boolean;

  /**
   * Return the wrapped version of this currency that can be used with the butter contracts. Currencies must
   * implement this to be used in butter
   */
  public abstract get wrapped(): Token;
}

```

## `NativeCurrency`
_NativeCurrency_代表它所在的链的主币
```typescript
export abstract class NativeCurrency extends BaseCurrency {
  public readonly isNative: true = true;
  public readonly isToken: false = false;
}
```
这里是一个EVM链上主币的实现 `EVMNativeCoin`

```typescript
/**
 * EVMNativCoin is the main usage of a 'native' currency, i.e. for Ethereum mainnet and all testnets
 */
export class EVMNativeCoin extends NativeCurrency {
  public constructor(
    chainId: string,
    decimal: number,
    symbol?: string,
    name?: string,
    logo?: string
  ) {
    super(chainId, decimal, ZERO_ADDRESS, symbol, name, logo);
  }

  public get wrapped(): Token {
    const weth9 = WCOIN(this.chainId);
    invariant(!!weth9, 'WRAPPED');
    return weth9;
  }

  public equals(other: Currency): boolean {
    return other.isNative && other.chainId === this.chainId;
  }
}
```
## `Token`
_Token_ 代表一个具有唯一地址和一些元数据的代币

```typescript
export class Token extends BaseCurrency {
  public readonly isNative: false = false;
  public readonly isToken: true = true;

  /**
   * The contract address on the chain on which this token lives
   */
  public override readonly address: string;

  public constructor(
    chainId: string,
    address: string,
    decimals: number,
    symbol?: string,
    name?: string,
    logo?: string
  ) {
    super(chainId, decimals, address, symbol, name, logo);
    this.address = validateAndParseAddressByChainId(address, chainId);
  }

  /**
   * Returns true if the two tokens are equivalent, i.e. have the same chainId and address.
   * @param other other token to compare
   */
  public equals(other: Currency): boolean {
    return (
      other.isToken &&
      this.chainId === other.chainId &&
      this.address === other.address
    );
  }

  /**
   * Return this token, which does not need to be wrapped
   */
  public get wrapped(): Token {
    return this;
  }
}
```

## `Chain`
代表一个区块链的结构
```typescript
export class Chain {
  /** chain id, we use string because some non-evm chain might have larger chainId that we defiend*/
  public readonly chainId: string;

  /** name of the chain */
  public readonly chainName: string;

  /** chain rpc uri */
  public readonly rpc?: string;

  /** chain scan url */
  public readonly scanUrl?: string;

  /** chain logo */
  public readonly chainLogo?: string;

  /** chain symbol **/
  public readonly symbol?: string;

  constructor(
    chainId: string,
    chainName: string,
    rpc?: string,
    scanUrl?: string,
    chainLogo?: string,
    symbol?: string
  ) {
    this.chainId = chainId;
    this.chainName = chainName;
    if (rpc) {
      this.rpc = rpc;
    }
    if (scanUrl) {
      this.scanUrl = scanUrl;
    }
    if (chainLogo) {
      this.chainLogo = chainLogo;
    }
    if (symbol) {
      this.symbol = symbol;
    }
  }
}
```