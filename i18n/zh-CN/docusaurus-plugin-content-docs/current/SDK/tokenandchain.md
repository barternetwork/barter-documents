---
sidebar_position: 3
---
# 代币和链
目前Butter只支持有限的代币和链，但是我们正在努力工作来支持更多的链和代币。

## 支持的代币
通过链ID来获得该链支持的Token列表
```typescript
const supportedTokenList: BaseCurrency[] = ID_TO_SUPPORTED_TOKEN('1')
```
上面的实例会返回Butter在以太坊主网(ChainId = 1)支持的所有代币。

## 支持的链
用下方Butter SDK中定义的常量来获得支持的主网列表
```typescript
SUPPORTED_CHAIN_LIST_MAINNET: Chain[]
```

测试网列表
```typescript
SUPPORTED_CHAIN_LIST_TESTNET: Chain[]
```