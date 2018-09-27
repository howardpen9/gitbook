---
description: 共識機制的討論
---

# \(補\)共識機制

## **問題**

在实际的计算机集群系统（看似强大的计算机系统，很多地方都比人类世界要脆弱的多）中，  
存在如下的问题：

* 节点之间的网络通讯是不可靠的，包括任意延迟和内容故障；
* 节点的处理可能是错误的，甚至节点自身随时可能宕机；
* 同步调用会让系统变得不具备可扩展性。

要解决这些挑战，愿意动脑筋的读者可能会很快想出一些不错的思路。

为了简化理解，仍然以两个电影院一起卖票的例子。可能有如下的解决思路：

* 每次要卖一张票前打电话给另外一家电影院，确认下当前票数并没超售；
* 两家电影院提前约好，奇数小时内一家可以卖票，偶数小时内另外一家可以卖；
* 成立一个第三方的存票机构，票都放到他那里，每次卖票找他询问；
* 更多……

这些思路大致都是可行的。实际上，这些方法背后的思想，**将可能引发不一致的并行操作进行串行化**，就是现在计算机系统里处理分布式一致性问题的基础思路和唯一秘诀。只是因为计算机系统比较傻，需要考虑得更全面一些；而人们又希望计算机系统能工作的更快更稳定，所以算法需要设计得再精巧一些。

## 挑戰

從各方面思考，



* **“Fault tolerance"** — decentralized systems are less likely to fail accidentally because they rely on manyseparate components that are not likely.
* **Attack resistance or seizure resistance** — decentralized systems are more expensive to attack anddestroy or manipulate because they lack sensitive central points that can be attacked at a much lowercost than the economic size of the surrounding system.
* **Collusion resistance** — it is much harder for participants in decentralized systems to collude to act inways that benefit them at the expense of other participants.
* **Censorship resistance** — this is one of the most important distinguishing factors of cryptocurrencies.Centralized systems are notoriously easy to censor. Censoring transactions allows for third parties toprevent the flow of transactions to or from certain sources.
* **Access control resistance** — Centralized services are easy to have access blocked by an ISP, government,or other authority. Even if the internet was completely shut down, however, blockchains could survivethrough satellites or shortwave radio.
* **Change resistance** — Blockchains are also incredibly resistant to change and are not subject to the whimsof a few individuals. Any major change requires consensus from the vast majority of participants.”



## 共識機制

共識機制分為幾種

以下將會區分成幾部分章節：

* 共識機制
* 演算法介紹
* 彼此差異



## 共識機制的種類：

* Proof of Work
* Proof of Stake
  * dPOS
* BFT
  * PBFT
  * dBFT
  * [Cosmos Consensus Algorithm](https://github.com/cosmos/cosmos/blob/master/WHITEPAPER.md)
  * [DEXON Consensus Algorithm](https://dexon.org/#section-top)

## PoW & PoS 





### Proof of Work 工作量證明

* 算力問題
* 


### Proof of Stake 權益投票

* 不需要浪費算力
* 權益\(資產\)愈多、愈願意去維護帳本的安全
* 51%算力攻擊的成本較高
* 
#### 缺失:

* 資本主義的延伸?  富者恆富



### 差異與優缺點







## BFT & DBFT 



### DEXON Consensus

* reliable broadcast
  * Total Ordering
  * Time Stamping

