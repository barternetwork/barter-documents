---
sidebar_position: 3
---
# Barter Omnichain Payment System(BOPS)

## About BOPS
Barter Omnichain Payment System(BOPS) is a tool that empowers Web3 applications with cross-chain payment as well as fiat payment functionality. With BOPS, application users can pay with whatever cryptocurrency they want, no matter what cryptocurrency is required by your application.  
For example, in your GameFi application, a user finds this amazing NFT on blockchain A that accepts only $tokenA as payment but he/she doesn't have any or doesn't have enough $tokenA. Then by [integrating BOPS](/How%20to%20Integrate/BOPS.md) into your GameFi application, the user can pay whatever cryptocurrency they want, say $tokenB on blockchain B, and BOPS will exchange $tokenB for $tokenA automatically to make the paynment or simply use the fiat payment service BOPS provided and pay by credit card that supports over 40 currencies.  
 In summary, BOPS makes it possible for applications to accept any cryptocurrency as payment and makes it possible for users to pay with any type of currency they want.  

![BOPS Comic](/img/barter/bops-comic.png "BOPS Comic")

## How BOPS Works
The diagram below shows the overall workflow when Alice purchased an item on the NEAR network by paying $ETH on the Ethereum Network.  


![BOPS Work Flow](/img/barter/BOPS-work-flow.png "BOPS Work Flow")
*Note: We highly recommend taking a look at [MAP Protocol Litebook](https://files.maplabs.io/pdf/mapprotocol_whitepaper_en.pdf) to fully understand how Barter works.*

It is impossible for our liquidity pool holds every single token pair, therefore we also offer cross-chain exchange aggregation by gathering all the liquidities of DEXs from the source chain, MAP Realy chain, and the target chain, then using our advanced smart route finding algorithm to find the best swap route for users. This way, we are able to cover all the token pairs as long as they are listed on major DEXs.

However, exchanging using an aggregator cost more than exchanging directly from our liquidity pool since DEX also charges a fee. Therefore, our [Shared Cross-chain Liquidity Pool](/Products/BSLP) provides interfaces for applications to directly add liquidities of their interests to provide users with the best fee rate.

