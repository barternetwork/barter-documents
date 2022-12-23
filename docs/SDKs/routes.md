---
sidebar_position: 8
---
# Route
Butter has built in smart router that will aggregate all the major dex's liquidity from source blockchain and target blockchain, to find the optimal swap route.

## Get the best route
```typescript
async function getBestRouteStr(
    fromToken: BaseCurrency,
    toToken: BaseCurrency,
    amountIn: string
): Promise<string> 
```
Example: get the best cross-chain route when swapping 1 BNB for MATIC

```typescript
const router = new ButterSmartRouter();

const routeStr = await router.getBestRoute(
    BNB_NATIVE,
    NEAR_NATIVE,
    ethers.utils.parseEther('1')
)

console.log('routeStr', routeStr);
```
The output would be:
```json
{
  "srcChain":[
    {
      "chainId":"56",
      "amountIn":"1",
      "amountOut":"159.366689922754163277",
      "path":[
        {
          "id":"0xc39F0738AD5852646c0C50d8e0D0F2216a9b58C8",
          "tokenIn":{
            "symbol":"BNB",
            "icon":"",
            "address":"0xae13d989daC2f0dEbFf460aC112a837C89BAa7cd"
          },
          "tokenOut":{
            "symbol":"BUSD",
            "icon":"",
            "address":"0x3F1E91BFC874625f4ee6EF6D8668E79291882373"
          }
        }
      ],
      "dexName":"Pancakeswap",
      "tokenIn":{
        "address":"0x0000000000000000000000000000000000000000",
        "decimals":18,
        "symbol":"BNB",
        "icon":""
      },
      "tokenOut":{
        "address":"0x3F1E91BFC874625f4ee6EF6D8668E79291882373",
        "name":"BUSD",
        "decimals":18,
        "symbol":"BUSD",
        "icon":""
      }
    }
  ],
  "mapChain":[
    {
      "chainId":"22776",
      "dexName":"",
      "amountIn":"159.366689922754163277",
      "amountOut":"159.266689922754163277",
      "tokenIn":{
        "address":"0x424D3bcdC96F42aC919F276D7D4f6C94f24e0703",
        "name":"map usdc",
        "decimals":18,
        "symbol":"mUSDC",
        "icon":""
      },
      "tokenOut":{
        "address":"0x424D3bcdC96F42aC919F276D7D4f6C94f24e0703",
        "name":"map usdc",
        "decimals":18,
        "symbol":"mUSDC",
        "icon":""
      },
      "path":[

      ]
    }
  ],
  "targetChain":[
    {
      "chainId":"80001",
      "amountIn":"159.266689922754163277",
      "amountOut":"160.99208763031242886",
      "path":[
        {
          "id":"0x60004720A9edeD2795A7429B11Dd8BcCE1A2358e",
          "tokenIn":{
            "name":"PolygonUSD",
            "symbol":"PUSD",
            "icon":"",
            "address":"0x1E01CF4503808Fb30F17806035A87cf5A5217727"
          },
          "tokenOut":{
            "name":"Wrapped MATIC",
            "symbol":"WMATIC",
            "icon":"",
            "address":"0x9c3C9283D3e44854697Cd22D3Faa240Cfb032889"
          }
        }
      ],
      "dexName":"Quickswap",
      "tokenIn":{
        "address":"0x1E01CF4503808Fb30F17806035A87cf5A5217727",
        "name":"PolygonUSD",
        "decimals":6,
        "symbol":"PUSD",
        "icon":""
      },
      "tokenOut":{
        "address":"0x0000000000000000000000000000000000000000",
        "decimals":18,
        "symbol":"MATIC",
        "icon":""
      }
    }
  ]
}
```
Now you get the best cross-chain swap route! This is used to execute a actual cross-chain swap and calculate swap fees.