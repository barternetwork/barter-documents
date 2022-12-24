---
sidebar_position: 2
---
# Butter桥与Butter Swap

## Butter桥
### 介绍
Butter桥以MAP Protocol作为跨链基础设施建造的一款跨连桥。它允许与所有链建立无障碍连接, 并且以一种真正的去中心化和无信任的方式将链上资产从一个区块链转移到另一个。

### Butter桥工作原理
下图显示了Alice将100个XYZ代币从以太坊转移到Near。
![Bridge Work Flow](/img/butter/bridge-flow.png "Bridge Work Flow")

我们可以看到，整个Bridge过程完全是在链上和链间完成的，没有任何外部验证者或人类参与。要了解更多信息，请访问 [MAP Protocol Website](https://www.maplabs.io).

## Butter Swap
### 介绍
Butter Swap可以让Web3应用和用户以最安全和最有效的方式交换跨链资产。
![bces comic](/img/butter/bces-comic.png "bces comic")

如今大多数跨链交易所和跨链桥都只支持稳定币兑换，这在一定程度上解决了流动性和无常损失的问题，但也带来了一个问题。假设爱丽丝想探索一个新的区块链，为了在该区块链上进行任何类型的操作，她将需要该链的原生币来消耗燃料费。而持有该区块链的稳定币是没有意义的，因为她不能用稳定币支付燃料费。
Butter不仅支持稳定币兑换，用户可以使用我们的服务兑换任何类型的代币。例如，直接用$ETH交换$NEAR。

### Butter Swap工作原理
Butter Swap有两种兑换方式:
#### **1. 直接兑换**  
直接兑换是交换跨链资产的默认方式，它允许用户通过Butter在MAP Relay Chain的流动性池直接用一种代币交换另一种代币。

![Core Flow](/img/butter/core.png "Core Flow")

#### **2. 聚合兑换**
然而，如果想通过Butter流动性池支持任意代币对几乎是不可能的，因此，如果用户想要交换不在我们流动性池中的代币对时，我们会使用Butter内置的跨链聚合器，收集来自不同区块链上的主要DEX的所有流动性，并使用寻路算法为任何代币对找到最佳跨链交换路线。

![Agg Flow](/img/butter/aggregator.png "Aggg Flow")


#### **3. Butter Swap 工作流程**
通过直接交换和聚合交换的结合，Butter Swap提供了最方便和全面的跨链交换服务。以下是Butter Swap的整体工作流程。

![Butter Swap Flow](/img/butter/flow.png "BCES Flow")