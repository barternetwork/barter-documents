---
sidebar_position: 2
---
# Butter Cross-chain Exchange Service(BCES)

## About BCES
Butter Cross-chain Exchange Service(BCES) gives Web3 applications the ability to exchange cross-chain assets in the most secure and efficient way. For example, one use case would be integrating Butter Cross-chain Exchange Service into crypto wallets, then their users can easily exchange cross-chain assets in the wallet they use every day. 
![bces comic](/img/butter/bces-comic.png "bces comic")

Most cross-chain exchanges and cross-chain bridges today support only stablecoin exchange, which solves the problem of liquidity and impermanent losses to a certain extend, but also poses a problem. Let's say Alice wants to explore a new blockchain, in order to perform any type of operation on that blockchain, she will need their native coin for gas consumption. And it is meaningless to hold that blockchain's stablecoin since she can't pay for the gas using stablecoin. 

However, BCES supports not only stablecoin exchange, users can exchange any type of token using our service. For example, directly swap $ETH for $NEAR, then they can freely explore Near's ecosystem.  

## How BCES works
There are two ways of exchanging using BCES:
### **1. Direct Exchange**  
Direct exchange is the default way of exchanging cross-chain assets, it allows users directly exchange one token for another through our liquidity pool. Also, Web3 applications are free to add any liquidity pair of their interests through our [Butter Shared Cross-chain Liquidity Pool](/Products/BSLP). 

![Core Flow](/img/butter/core.png "Core Flow")

### **2. Aggregation Exchange**
However, it is nearly impossible to hold every token pair in our liquidity pool, thus, in case users want to exchange token pair that is not in our liquidity pool, we have a cross-chain aggregator that gathers all liquidities from major DEXs on different blockchains and uses an advanced route-finding algorithm to find the best cross-chain exchange route for any token pair. 

![Agg Flow](/img/butter/aggregator.png "Aggg Flow")


### **3. BCES workflow**
By combining direct exchange and aggregation exchange, BCES provides the most convenient and comprehensive cross-chain exchange service. Here is the overall workflow of BCES.

![BCES Flow](/img/butter/flow.png "BCES Flow")