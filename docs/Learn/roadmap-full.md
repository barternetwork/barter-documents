# Roadmap

#### 2022 Q2

- Build a single-chain smart router that supports major DEXs on major blockchains respectively.
  - Design smart router's architecture and code structure for a high-performance and easy-to-scale service.
  - Integrate the following major DEXs on Ethereum, BSC, Avalanche, Solana, Polygon, Fantom, and Arbitrum into the smart router.
    - Curve
    - Uniswap
    - Pancakeswap
    - Sushiswap
    - Balancer
    - Platypus
    - Quickswap
    - Trader Joe
    - Bancor
    - BiSwap
    - Meshswap
    - Raydium
    - Serum
  - Design and implement a unified pool and route structure for all DEXs above.
  - Design a routing algorithm to support split-route swapping based on all route paths from different DEXs on the same network only.
  - Unit testing, aggregation testing, and stress testing.
- Develop Barterswap smart contracts that enable one-step aggregate swapping between major DEXs on major blockchains, respectively.
  - Design code structure to allow dynamic expansion for more DEX safely and efficiently.
  - Different DEX has different exchange logic; integrate them into the Barter's router contract for each chain.
  - Unit testing and aggregation testing.
- Develop a high-performance, high-availability back-end server, which constantly fetches major DEXs’ liquidity information to support smart route-finding.  
  - Design unified pool data structure: Different DEXs' pool has their unique pool data structure, adapting all format into a unified format.
  - Get pool data(pool address, reserves, token information, etc.) from each DEX's subgraph server.
  - Get on-chain pool data because the subgraph server may not be stable, and there is always a delay with block synchronization.
  - Design a scheduled task that will constantly acquire the pool data from the two data sources above and write to a high-performance database for persistent storage. 
  - Design and implement server API for other services to fetch pool data.
  - Unit testing, aggregation testing, and stress testing.

#### 2022 Q3

- Build Barterswap Core using a beta version bridge to support cross-chain liquidity providing and exchanging between popular EVM based chains
  - Use a beta-version bridge to connect EVM-based chains like Ethereum, BSC, Avalanche, Fantom, and Polygon.
  - Write and deploy Barter Core smart contracts for cross-chain liquidity providing and exchanging.
- Build Barter Aggregator that optimizes cross-chain route-finding between popular EVM based chains
  - Adjust smart router to support cross-chain exchange route-finding among all routes from different chains. 

#### 2022 Q4

- Launch Barter Bridge (a truly decentralized cross-chain bridge with maximum security) upon Map Protocol that connects to Ethereum and Near.
  - Develop vault contract on Ethereum, Near, and Map Protocol respectively to store assets securely.
  - Develop messenger - an inter-chain program that sends messages across different chains.
- Launch Barter Core to support cross-chain liquidity providing and swapping on Ethereum and Near through Barter Bridge; for other popular EVM-based chains like BSC, Polygon, Fantom, etc., use Barter Bridge beta first.
  - Develop core contracts supports directly multi-chain liquidity adding and exchanging.
- Launch Barter Aggregator that finds the best trade route for cross-chain swapping between popular EVM-based chains and Near chain.
- Audit smart contracts with at least two auditors.

#### 2023 Q1

- Extend Barter Bridge to support popular EVM-based chains.
- For those chains that are using Barter Bridge Beta at this point, replace with Barter Bridge.
- Extend Barter Core using Barter Bridge Beta to connect to Solana, Arbitrum, Optimism, etc.
- Extend Barter Aggregator to support Solana, Near, Arbitrum, Optimism, etc.

#### 2023 Q2

- Extend Barter Bridge to support Solana, Near, Arbitrum, Optimism, etc.
- For those chains that are using Barter Bridge Beta at this point, replace with Barter Bridge.
- At this time, Barterswap will support cross-chain exchange for most popular blockchains and will continue connecting all chains that are new rise up.