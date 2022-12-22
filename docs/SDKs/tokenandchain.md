# Tokens and Chains
Currently Butter only support limited tokens and chains, but we are working hard to cover more right now!
## Supported Tokens
To get supported tokens by chain, use
```typescript
const supportedTokenList: BaseCurrency[] = ID_TO_SUPPORTED_TOKEN('1')
```
This will return list of supported token on Ethereum Mainnet.

## Supported Chains
To get supported chains on mainnet, use the constant defined in the SDK
```typescript
SUPPORTED_CHAIN_LIST_MAINNET: Chain[]
```

To get supported chains on testnet, use the constant defined in the SDK
```typescript
SUPPORTED_CHAIN_LIST_TESTNET: Chain[]
```