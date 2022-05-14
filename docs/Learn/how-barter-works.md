#  How Barterswap Works

Before diving into how Barterswap works, let's first understand its components.  

## Barterswap Components
Barterswap consists of three components.  

1. Barter Bridge - A cross-chain bridge developed upon MAP Protocol.
2. Barter Core - A pioneering cross-chain swapping implementation.
3. Barter Aggregator - Find the best cross-chain swap route among major DEXs. 

### Barter Bridge

Barter Bridge is a cross-chain bridge built upon MAP protocol. It allows the establishment of connections with all chains barrierless. In short, Barter Bridge allows users to move tokens from one chain to another in a truly decentralized way. The bridge process is fast, cheap, and ensures maximum security thanks to MAP Protocol.

### Barter Core
![Barter Core](/img/barter/core.png "Bridge Core")

Barter Core is a pioneering cross-chain exchange implementation deployed on MAP Protocol's Relay Chain. It allows Barterswap's liquidity pool to hold tokens from different chains such as $AVAX and $NEAR. This also means users can directly exchange tokens from different chains in one liquidity pool.  

### Barter Aggregator  

![Barter Aggregator](/img/barter/aggregator.png "Bridge Aggregator")
Barter Aggregator uses the most advanced price tracing algorithm to find the best cross-chain exchange route based on major DEXs on the market.

## Cross-Chain Exchange Process
The diagram below shows how Barterswap really works.
![Barter Flow](/img/barter/flow.png "Bridge Flow")  
Let's assume Alice wants to swap some of her $AVAX on Avalanche for some $NEAR on Near. There are two ways of exchange using Barterswap:
1. Direct Swap (Default)  
First of all, $AVAX token will be wrapped as $mAVAX (a MAP representation of $AVAX) through Barter Bridge. Then $mAVAX will be exchanged through $mAVAX-$mNEAR liquidity pool for $mNEAR. Finally, unwrap $mNEAR through Barter Bridge and send $NEAR to Alice.
2. Aggregation Swap  
If unfortunately, there is no $mAVAX-$mNEAR pool, Barterswap will use Barter Aggregator to consider major DEXs on Avalanche, all liquidity pools on Barterswap, and major DEXs on NEAR together, using our advanced algorithm to find the best trade route for Alice. In this case, Barterswap will swap $AVAX for $avaxUSDC on let's say, Trader Joe. Then wrap $avaxUSDC as $mUSDC, unwrap $mUSDC to $nearUSDC, and finally swap $nearUSDC for $NEAR through Near's DEX.
