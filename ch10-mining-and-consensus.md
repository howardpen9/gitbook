# \(待\)Ch10\_Mining & Consensus

## 

## 簡介

"挖礦"兩個字其實有一些誤解。雖然整體BTC網路的激勵制度、促使礦工進行維護與運作的原因是**計算出新區塊**而產生的獎勵，倒也是有點類似於現實中採集稀有金屬的一段過程。

**而挖礦\(mining\)最重要的是誘因\(incentives\)，以及整體系統的激勵制度\(mechanism\)。**

> **如果你僅將挖礦的行為、視為創建新幣的過程，  
> 那麼你便是錯把手段\(激勵制度\)當成目的。**

挖礦\(mining\)保證了BTC系統的安全，確保整個廣大的網路不需要任何中央機構的授權。"鑄造新幣\(minted coins\)" 以及 "交易手續費"是一種激勵制度的設計，促使礦工們的行為、與網路的安全保持一致；或是說，處在同一條船上，往同一個方向前進，並且確保貨幣的供應量。

礦工驗證交易，並且將他們紀錄在全球性的帳本上；包含從一開始的區塊內交易紀錄、直到當下，並且維持每十分鐘一個出塊\(挖礦\)的時間。

{% hint style="info" %}
挖礦幾乎是BTC系統得以運行下去、最重要的動力之一。因為每個礦工都為了爭取獎勵而共同維護帳本的安全，並且隨著投入愈多、愈不可能做亂以及造成整體效益降低的事。

_The purpose of mining is not the creation of new bitcoin. That’s the **incentive system**. Mining is the **mechanism** by which bitcoin’s security is decentralized._
{% endhint %}

總結來講，礦工的收益分為兩種、除了打包區塊所得到的獎勵以外，另外一個便是交易手續費\(transaction fee\)。交易手續費為"該次"打包區塊內所有的交易手續費加總。而為了得到獎勵，礦工得解決一道數學難題\(一個基於加密哈希算法的難題\)。_**\*A cryptographic hash algorithm.**_

而解決這道數學難題仰賴的是電腦的計算能力、依據工作量的證明\(POW\)多寡來判斷。而礦工們彼此藉著POW競爭"紀錄交易"好獲得區塊獎勵，在區塊鏈系統中、是讓比特幣這套貨幣系統得以安全的重要基礎。

### Mining? 挖礦?

至於這樣的過程為何被稱作挖礦\(mining\)? 大概是起因於獎勵新生成的貨幣會是遞減收益的模型、就好像開採金屬一樣。根據每次產出50、每四年減半一次的數學函式，直到2140年總量 20,999,999.98的BTC將會被產出完畢。

考量交易手續費，直到目前為止、**手續費約略占比特幣收入的0.5%**或是更少。絕大部分收入來自新鑄造的比特幣。

#### Bitcoin Economics & Currency Creation - 貨幣的產量、產出的數學模型

每210,000個區塊高度後，產量減半；也就是約**每四年減半一次**。根據等比減半的概念\(好比摺紙一樣\)，隨者減半次數越多，打包新區塊的獎勵將會近似於 "1 satoshis"

![Supply of bitcoin currency over time based on a geometrically decreasing issuance rate](.gitbook/assets/image.png)

藉由限額的增發函式、提供了一個抗通膨的貨幣模型。不像法幣貨幣礙於中央機構的設計、造成無限量的增發情形。

{% hint style="success" %}
 [Why 每210,000個區塊後，產量減半?](https://cryptopeng.gitbook.io/blockchain/~/drafts/-LNZdGJxdP6bXD6P2mSO/primary/bitcoin-information#mei-210-000-liang-ban)
{% endhint %}

## Deflation Money - 通貨緊縮的錢

固定的流通量、與逐漸減少的貨幣發行，最有爭議的無非就是**通貨緊縮**、以及貨幣具有某種程度的**蒐藏\(儲藏\)價值**。

通貨緊縮也可以理解為，是因為流通量不匹配需求、所導致的**供需失衡**，所造成的升值現象。且貨幣的價值往往會因為時間的拉長、而增長。\(放愈久愈值錢啦!\)

#### 通貨緊縮 vs 通貨膨脹

然而，許多經濟學家都對於通貨緊縮的貨幣系統提出質疑。因為隨著時間推移，民眾傾向於持有貨幣、而非花費與使用它。

而BTC的專家則提出疑問，認為通貨緊縮可能是適當的；因為在"Fiat"的世界裡面，無上限的印製鈔票的誘因、是難以讓這套系統維持在通縮的模型的，不可能。

而BTC的產出既簡單、又合乎可預測的方式，按照一種穩定、可預期的供應量增發。

#### **在政府的控制下，貨幣遭受"債務發行"的道德風險；對於政府來說、可以在日後通過犧牲儲戶的資產為代價\(透過貶值、增稅\)，來消除任何對自身不利的風險。**

> Currencies under government control suffer from the moral hazard of easy debt issuance that can later be erased through debasement at the expense of savers.

不過這方面其實仍舊有待爭議的。對於通貨緊縮的系統來說，其是否擁有絕對性的問題這點仍有待觀察。

因為當我們不是由快速的經濟成長來驅動貨幣系統時，對通貨膨脹與貨幣貶值的保護、遠遠超過通貨緊縮的風險。

> It remains to be seen whether the deflationary aspect of the currency is a problem when it is not driven by rapid economic retraction, or an advantage because the protection from inflation and debasement far outweighs the risks of deflation.

## Decentralized Consensus - 去中心化共識

在前一個章節\(CH9\)我們嘗試將區塊鏈世界視為一個巨型帳本；且在這全球性且任何人都可以參與紀錄的Bitcoin網路，它登記了全部的所有權紀錄。

**但我們如何確保交易是"真實的"? 如何確保A真的擁有這些數量的BTC?**

在傳統的支付系統當中，都仰賴信任的機構以及任何第三方系統，來驗證或清算\(結算\)所有的交易。然而BTC系統當中被沒有任何中央機構，且倘若任何人手中都有一個帳本、那麼便不需要信任獨立的第三方機構了，每個人都可以見證與驗證所有權的轉移。

#### 區塊鏈網路中，每個節點都是一個獨立的個體。

中本聰最偉大的發明是設計出一個去中心的機制，且依就可以運行的新型態共識機制\(emergent consensus\)。

成千上萬個節點之間的同步性是不可能百分百達到的！要解決這個問題的最好辦法就是大家遵守同樣的規則。

BTC的去中心化共識機制相互作用有以下四個程序:

* 每個節點、都獨立驗證每一筆傳送交易\(Tx\)；按照一個完整的列表
* 通過挖礦節點，通過PoW、將這些傳送交易\(Tx\)獨立聚合到新的區塊
* 每個節點、都能獨立驗證新的區塊加入到鏈上
* 通過工作量證明，每個節點都會選擇具有最多計算力\(Computation\)的鏈

## Independent Verification of Transactions

在交易的章節有提過些許的細節**`[Transactions]`**。  
簡單的來說，BTC網路以及交易系統，便是收集所有的UTXO彙總起來，就是這套貨幣系統。

提供合適的解鎖腳本、並能將鎖定中的比特幣進行轉移，轉移給新得持有者。而最後的交易結果\(最後誰持有什麼\)、便會廣播出去給周遭節點，逐漸擴散出去。

想當然耳，在廣播出去最終交易結果給周遭鄰居節點前，每個BTC節點都會驗證所收到的交易\(Transaction\)是否有效。確保永遠只有驗證通過、有效\(valid\)的交易會被廣播出去。而無效的交易事務\(transactions\)在交給他們第一個節點時會被丟棄。

每個節點都根據這個標準清單程序表來驗證每個交易事務\(transaction\):

* 交易\(tx\)的語法、以及數據結構必須正確。
* 輸入或輸出不能任意為空。\(都要有數字/數量\)
* 交易的大小\(size\)必須小於 MAX\_BLOCK\_SIZE\(區塊可塞進的最大容量\)。
* 每個Output的加總、必須小於21M Coins；大於 dust threshold
*  None of the inputs have hash=0, N=–1  \(coinbase transactions should not be relayed\).
* 














