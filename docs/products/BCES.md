---
sidebar_position: 2
---
# Barter Cross-chain Exchange Service(BCES)

## About BCES
Barter Cross-chain Exchange Service(BCES) gives Web3 applications the ability to exchange cross-chain assets in the most secure and efficient way. For example, one use case would be integrating Barter Cross-chain Exchange Service into crypto wallets, then their users can easily exchange cross-chain assets in the wallet they use every day. 
![bces comic](/img/barter/bces-comic.png "bces comic")

Most cross-chain exchanges and cross-chain bridges today support only stablecoin exchange, which solves the problem of liquidity and impermanent losses to a certain extend, but also poses a problem. Let's say Alice wants to explore a new blockchain, in order to perform any type of operation on that blockchain, she will need their native coin for gas consumption. And it is meaningless to hold that blockchain's stablecoin since she can't pay for the gas using stablecoin. 

However, BCES supports not only stablecoin exchange, users can exchange any type of token using our service. For example, directly swap $ETH for $NEAR, then they can freely explore Near's ecosystem.  

## How BCES works
There are two ways of exchanging using BCES:
### **1. Direct Exchange**  
Directly exchange one token for another as long as our liquidity pool hold that certain token pair. Also, Web3 applications are free to add any liquidity pair they want through our [Barter Shared Cross-chain Liquidity Pool](/Products/BSLP). 

![Core Flow](/img/barter/core.png "Core Flow")

### **2. Aggregation Exchange**
No certain liquidity pair? No problem, BCES also offers a cross-chain exchange aggregator. Our aggregator gathers all the liquidities from all major DEXs from different blockchains and uses an advanced price tracing algorithm to find the best cross-chain exchange route.

![Agg Flow](/img/barter/aggregator.png "Aggg Flow")


### **3. BCES workflow**
By combining direct exchange and aggregation exchange, BCES provides the most convenient and comprehensive cross-chain exchange service. Here is the overall workflow of BCES.

![BCES Flow](/img/barter/flow.png "BCES Flow")