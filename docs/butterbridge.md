---
sidebar_position: 2
---
# Butter Bridge/Swap

## Butter Bridge
### About Butter Bridge
Butter Bridge is built on MAP Protocol, which is the foundation of all Butter's products. It allows the establishment of connections with all chains barrierless. In short, Butter Bridge allows users to move on-chain assets from one blockchain to another in a truly decentralized and trustless way.

### How Butter Bridge Works
Let's see how Butter Bridge works internally through Map Protocol. The diagram below shows Alice bridging 100 $XYZ from Ethereum to Near.
![Bridge Work Flow](/img/butter/bridge-flow.png "Bridge Work Flow")

As we can see, the whole bridging process is done on-chain and inter-chain solely without any external validator or human being involved. To learn more, please visit [MAP Protocol Website](https://www.maplabs.io).
## Butter Swap
### About Butter Swap
Butter Service gives Web3 applications the ability to exchange cross-chain assets in the most secure and efficient way. For example, one use case would be integrating Butter Cross-chain Swap Service into crypto wallets, then their users can easily exchange cross-chain assets in the wallet they use every day. 
![bces comic](/img/butter/bces-comic.png "bces comic")

Most cross-chain exchanges and cross-chain bridges today support only stablecoin exchange, which solves the problem of liquidity and impermanent losses to a certain extend, but also poses a problem. Let's say Alice wants to explore a new blockchain, in order to perform any type of operation on that blockchain, she will need their native coin for gas consumption. And it is meaningless to hold that blockchain's stablecoin since she can't pay for the gas using stablecoin. 

However, Butter supports not only stablecoin exchange, users can exchange any type of token using our service. For example, directly swap $ETH for $NEAR.

### How Butter Swap works
There are two ways of exchanging using Butter Swap:
#### **1. Direct Swap**  
Direct exchange is the default way of exchanging cross-chain assets, it allows users directly exchange one token for another through our liquidity pool.

![Core Flow](/img/butter/core.png "Core Flow")

#### **2. Aggregation Swap**
However, it is nearly impossible to hold every token pair in our liquidity pool, thus, in case users want to exchange token pair that is not in our liquidity pool, we have a cross-chain aggregator that gathers all liquidity from major DEXs on different blockchains and uses an advanced route-finding algorithm to find the best cross-chain exchange route for any token pair. 

![Agg Flow](/img/butter/aggregator.png "Aggg Flow")


#### **3. Butter Swap workflow**
By combining direct exchange and aggregation exchange, Butter Swap provides the most convenient and comprehensive cross-chain exchange service. Here is the overall workflow of Butter Swap.

![Butter Exchange Flow](/img/butter/flow.png "BCES Flow")