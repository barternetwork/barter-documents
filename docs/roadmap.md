# Roadmap
#### 2022 Q2

- Build a single-chain smart router that supports major DEXs on major blockchains respectively.
  - Design smart router's architecture and code structure for a high-performance and easy-to-scale service.
  - Integrate the major DEXs on Ethereum, BSC, Near, Harmony, Avalanche, Klaytn, Solana, Polygon, and Fantom into our smart router.

  - Design and implement a unified pool and route structure for all DEXs integrated.
  - Design a routing algorithm to support split-route swapping based on all route paths from different DEXs on the same network only.
- Developing smart contracts that enable one-step aggregate swapping between major DEXs on major blockchains, respectively.
  - Design code structure to allow dynamic expansion for more DEX safely and efficiently.
  - Different DEX has different exchange logic; integrate them into the Butter's router contract for each chain.
- Develop a high-performance, high-availability back-end server, which constantly fetches major DEXs’ liquidity information to support smart route-finding.  
  - Design unified pool data structure: Different DEXs' pool has their unique pool data structure, adapting all format into a unified format.
  - Get pool data(pool address, reserves, token information, etc.) from each DEX's subgraph server.
  - Get on-chain pool data because the subgraph server may not be stable, and there is always a delay with block synchronization.
  - Design a scheduled task that will constantly acquire the pool data from the two data sources above and write to a high-performance database for persistent storage. 
  - Design and implement server API for other services to fetch pool data.

#### 2022 Q3

- Develop Butter Bridge that connects to BSC, and a few other chains that are TBD
- Develop Butter Cross-chain Exchange, Butter Omnichain Payment System, Butter Chain Switcher and Butter Shared Cross-chain Liquidity Pool.
- Build Butter Aggregator that optimizes cross-chain route-finding between those chains.
  - Adjust smart router to support cross-chain exchange route-finding among all routes from different chains. 

#### 2022 Q4

- Launch Butter Bridge which is built upon Map Protocol that connects to Ethereum and few other chains that are TBD.
  - Develop vault contracts.
  - Develop messenger - an inter-chain program that sends messages across different chains.
- Launch ButterJS SDK that connects to supported chains.
- Testing, testing and testing.
- Smart contract audit.


#### 2023 Q1 and further
Keep connecting to major blockchains, each blockchain requires the following work:
- Extend cross-chain aggregator to support more chains.
- Write on-chain vault smart contracts and inter-chain messenger program for each blockchain connected.
- Write one-step aggregation exchange smart contracts for each blockchain connected.
- Extend ButterJS SDK to support more blockchains.
- Testing and smart contract audit.
