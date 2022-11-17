#  How Butterswap Works

Before diving into how Butterswap works, let's first understand its components.  

## Butterswap Components
Butterswap consists of three main components.  

* Butter Bridge - A cross-chain bridge developed upon MAP Protocol.
* Butter Core - A pioneering cross-chain swapping implementation.
* Butter Aggregator - A smart routing service that finds the best cross-chain route among major DEXs. 

### Butter Bridge

Butter Bridge is a cross-chain bridge built upon MAP protocol. It allows the establishment of connections with all chains barrierless. In short, Butter Bridge allows users to move tokens from one chain to another in a truly decentralized way. The bridge process is fast, cheap, and ensures maximum security thanks to MAP Protocol.  
Butter Bridge ensures maximum security, we used blockchain-level security to ensure there is absolutely no risk for users' assets, which no other cross-chain exchange is able to guarantee.

### Butter Core
![Butter Core](/img/butter/core.png "Bridge Core")

Butter Core is a pioneering cross-chain exchange implementation deployed on MAP Protocol's Relay Chain. It allows Butterswap's liquidity pool to hold tokens from different chains such as $AVAX and $NEAR. This also means users can directly exchange tokens from different chains in one liquidity pool. Better yet, Butterswap guarantees the lowest fee rate with only a 0.4% fee for cross-chain swap, which by far is the lowest compared to others.

### Butter Aggregator  

![Butter Aggregator](/img/butter/aggregator.png "Bridge Aggregator")
Butter Aggregator uses the most advanced price tracing algorithm to find the best cross-chain exchange route based on major DEXs on the market.

## Cross-Chain Exchange Process
The diagram below shows how Butterswap really works.
![Butter Flow](/img/butter/flow.png "Bridge Flow")  
Let's assume Alice wants to swap some of her $AVAX on Avalanche for some $NEAR on Near. There are two ways of exchange using Butterswap:
1. Direct Swap (Default)  
First of all, $AVAX token will be wrapped as $mAVAX (a MAP representation of $AVAX) through Butter Bridge. Then $mAVAX will be exchanged through $mAVAX-$mNEAR liquidity pool for $mNEAR. Finally, unwrap $mNEAR through Butter Bridge and send $NEAR to Alice. And Butterswap only charges a 0.4% fee for the whole process.
2. Aggregation Swap  
If unfortunately, there is no $mAVAX-$mNEAR pool, Butterswap will use Butter Aggregator to consider major DEXs on Avalanche, all liquidity pools on Butterswap, and major DEXs on NEAR together, using our advanced algorithm to find the best trade route for Alice. In this case, Butterswap will swap $AVAX for $aUSDC on let's say, Trader Joe. Then wrap $aUSDC as $mUSDC, unwrap $mUSDC to $nUSDC, and finally swap $nUSDC for $NEAR through Quickswap.
