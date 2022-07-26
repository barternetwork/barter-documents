# Barter Omnichain Payment System(BOPS)

## TL;DR
A peer-to-peer decentralized payment systems are the origin of blockchain technology, and one of the most common use cases. The idea is to eliminate third-party entities from the payment process and allow people around the world to participate in a trustless, distributed network. However, the overgrowing number of blockchains and cryptocurrencies has resulted in a major liquidity fragmentation. To solve this problem, cross-chain interoperability seems to be the solution. The problem is that cross-chain exchanges and bridges have a high threshold for users, first, the cross-chain process is very complicated, and second, users could put their assets at risk if not chosen carefully. And yet, why should web3 users need to go through such a complicated process and careful research to enjoy cross-chain technology? We don't see every single person using the internet understanding TCP/IP, we don't see every single person using social media understanding the content-based recommendation algorithm, then why on earth web3 users should ever be aware of the existence of cross-chain technology? Thus, with this goal in mind, we created Barter Omnichain Payment System to let projects provide their users with a seamless cross-chain experience.
<br> 
## About BOPS
Barter Omnichain Payment System(BOPS) is a tool that empowers web3 applications with cross-chain payment as well as fiat payment functionality. With BOPS, application users can pay with whatever cryptocurrency they want, no matter what cryptocurrency is required by your application. For example, in your GameFi application, a user finds this amazing NFT on blockchain A that accepts only \$tokenA as payment but he/she doesn't have any or doesn't have enough \$tokenA. Then by __integrating BOPS__ into your GameFi application, the user can pay whatever cryptocurrency they want, say \$tokenB on blockchain B, and BOPS will exchange \$tokenB for \$tokenA automatically. Or simply use the BOPS fiat payment service and pay by credit card that supports over 40 currencies. Better yet, in case your user doesn't hold any native token on the blockchain your application deployed on, Barter also offers a __cross-chain exchange service__ that will exchange any token users hold on any blockchain for a certain native token for gas consumption. In summary, BOPS makes it possible for applications to accept any cryptocurrency as payment and makes it possible for users to pay with any type of currency they want.
<br> 
## Technical Overview
### Powered by MAP Protocol
Security has always been the biggest critique on the whole cross-chain industry, Barter fully understands the importance of security and that is why we use __MAP Protocol__ as our underlying cross-chain infrastructure. MAP Protocol is simply the most secure and comprehensive cross-chain solution out there. MAP Protocol uses an independent network of self-verified light clients deployed on each blockchain for cross-chain verification, which guarantees blockchain-level security and ensures everything is controlled by codes. We believe the human being is not to be trusted, but code doesn't lie, that's why we are built on the best cross-chain solution that is truly trustless and decentralized. To learn more about MAP Protocol, you can visit the link [here](https://files.maplabs.io/pdf/mapprotocol_whitepaper_en.pdf).

### How BOPS Works
The diagram below shows the overall workflow when Alice wants to buy an item on the NEAR network but she only has $ETH.
![BOPS Work Flow](/img/logo.png "BOPS Work Flow")
We highly recommend taking a look at __MAP Protocol Litebook__ to fully understand how Barter works.

It is impossible for our liquidity pool holds every single token pair, therefore we also offer cross-chain exchange aggregation by gathering all the liquidities of DEXs from the source chain, MAP Realy chain, and the target chain, then using our advanced smart route finding algorithm to find the best swap route for users. This way, we are able to cover all the token pairs as long as they are listed on major DEXs.

However, exchanging using an aggregator cost more than exchanging directly from our liquidity pool since DEX also charges a fee. Therefore, our __Shared Cross-chain Liquidity Pool__ provides interfaces for applications to directly add liquidities of their interests to provide users with the best fee rate.
<br>

## How to Integrate
coming soon...
