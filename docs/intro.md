---
sidebar_position: 1
slug: /
---
# Barterswap Technical Overview

Barterswap is the unified and limitless cross-chain exchange built upon [MAP Protocol](https://www.maplabs.io/). Theoretically, it is able to connect to all Turing-Complete chains through MAP Relay Chain.

## Barter Aggregator

Barter Aggregator uses the most advanced price tracing algorithm to find the best cross-chain exchange route based on major DEXs on the market. It contains three components: a back-end server, exchange smart contracts, and a smart router.

#### Back-End Server

Barter Aggregator contains a server that constantly fetches and updates major DEX's pool data like pool address, reserves, token data, etc. The server will acquire data from two sources: subgraph and on-chain data, to obtain the most up-to-date data.

#### Smart Router

Barter Smart Router uses the most advanced price tracing algorithm to find the best cross-chain exchange route for trade. It will gather all major DEX's pool data from the server, then find all the routes from the input token to the output token, split the swap amount into different distributions, and finally, among all the routes with different distributions, find the best route for the trade considering gas fee and exchange fee.

#### Smart Contracts

A set of smart contracts that integrate all popular DEX's core contracts to perform exchange operations within one single transaction.



## Barter Bridge

Barter Bridge uses MAP Protocol as the underlying infrastructure. It is responsible for bridging the source token from the source chain to the MAP representation of the token on the MAP relay chain and vice-versa.
![Barter Bridge](/static/img/barter/bridge-detail.png "Bridge Illustration")
The above diagram shows the complete exchange process of Alice swapping 100 $ETH on Ethereum for 100 $ETH on Solana. Let's see how Barter Bridge works internally through Map Protocol.

1. Barter will lock the $ETH to the vault contract deployed on Ethereum.
2. The vault contract emits a lock event.
3. Messenger - an inter-chain program between Ethereum and MAP Protocol, listens to Lock event and builds proof on Ethereum.
4. Messenger transfer the message to vault contract resides on MAP Relay Chain.
5. Vault contract on MAP Relay Chain verifies the message just received against the light client deployed on Ethereum.
6. If the message is verified successfully, mint 100 $mETH - a MAP representation of ETH.
7. Burn the 100 $ETH we just minted.
8. Messenger between MAP Relay Chain and Solana listens to the burn event and builds proof on MAP Relay Chain.
9. Messenger transfer the message to the vault contract on Solana.
10. Vault on Solana verifies the message just received against the light client deployed on MAP Relay Chain.
11. If the message is verified successfully, transfer 100 $ETH on Solana to Alice.

For more information on vault, messenger, and light-client, please see [MAP Protocol Litebook](https://files.maplabs.io/pdf/mapprotocol_whitepaper_en.pdf).

## Barter Core

Barter Core is an exchange implementation deployed on MAP Protocol's Relay Chain; it allows the direct exchange of tokens from different chains.

![Barter Core](/static/img/barter/core.png "Bridge Core")

#### Adding Liquidity

Barter Core allows a liquidity pool holding tokens from different chains by simply wrap the token as mTokens through Barter Bridge, essentially all tokens in Barterswap's liquidity pool are MAP presentations of that certain token.



#### Direct Exchange

Directly exchange one token for another in Barterswap liquidity pool.
