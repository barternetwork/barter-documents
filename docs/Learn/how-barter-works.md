#  How Barterswap Works

Before diving into how Barterswap works, let's first understand its components.  

## Barterswap Components
Barterswap consists of three main components.  

* Barter Bridge - A cross-chain bridge developed upon MAP Protocol.
* Barter Core - A pioneering cross-chain swapping implementation.
* Barter Aggregator - A smart routing service that finds the best cross-chain route among major DEXs. 

### Barter Bridge

Barter Bridge is a cross-chain bridge built upon MAP protocol. It allows the establishment of connections with all chains barrierless. In short, Barter Bridge allows users to move tokens from one chain to another in a truly decentralized way. The bridge process is fast, cheap, and ensures maximum security thanks to MAP Protocol.  
Barter Bridge ensures maximum security, we used blockchain-level security to ensure there is absolutely no risk for users' assets, which no other cross-chain exchange is able to guarantee.

### Barter Core
![Barter Core](/static/img/barter/core.png "Bridge Core")

Barter Core is a pioneering cross-chain exchange implementation deployed on MAP Protocol's Relay Chain. It allows Barterswap's liquidity pool to hold tokens from different chains such as $AVAX and $NEAR. This also means users can directly exchange tokens from different chains in one liquidity pool. Better yet, Barterswap guarantees the lowest fee rate with only a 0.4% fee for cross-chain swap, which by far is the lowest compared to others.

### Barter Aggregator  

![Barter Aggregator](/static/img/barter/aggregator.png "Bridge Aggregator")
Barter Aggregator uses the most advanced price tracing algorithm to find the best cross-chain exchange route based on major DEXs on the market.

## Cross-Chain Exchange Process
The diagram below shows how Barterswap really works.
![Barter Flow](/static/img/barter/flow.png "Bridge Flow")  
Let's assume Alice wants to swap some of her $AVAX on Avalanche for some $NEAR on Near. There are two ways of exchange using Barterswap:
1. Direct Swap (Default)  
First of all, $AVAX token will be wrapped as $mAVAX (a MAP representation of $AVAX) through Barter Bridge. Then $mAVAX will be exchanged through $mAVAX-$mNEAR liquidity pool for $mNEAR. Finally, unwrap $mNEAR through Barter Bridge and send $NEAR to Alice. And Barterswap only charges a 0.4% fee for the whole process.
2. Aggregation Swap  
If unfortunately, there is no $mAVAX-$mNEAR pool, Barterswap will use Barter Aggregator to consider major DEXs on Avalanche, all liquidity pools on Barterswap, and major DEXs on NEAR together, using our advanced algorithm to find the best trade route for Alice. In this case, Barterswap will swap $AVAX for $aUSDC on let's say, Trader Joe. Then wrap $aUSDC as $mUSDC, unwrap $mUSDC to $nUSDC, and finally swap $nUSDC for $NEAR through Quickswap.
