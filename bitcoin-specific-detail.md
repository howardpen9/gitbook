---
description: 關於BTC運作的特定細節，以及實際上如何運作。
---

# Bitcoin的特定細節

## 節點認證

從第一個區塊到各分支末端中所有區塊難度總和最高的分支稱為主分支，通常也是分支長度最長的。[區塊要再經過100 次挖到礦後才被認為成熟](https://en.bitcoin.it/wiki/Confirmation)，只有主分支上的成熟區塊才能獲得獎勵。

每筆交易被包在區塊中廣播出來就相當於一次確認，該區塊之後每多串連一個區塊也多一次確認。一般交易所或電子錢包的**交易都會要求6 次確認以上\(約略10分鐘出一次塊、6次確認恰為一小時\)。**

因為假設在網路暢通下，不誠實的礦工有雜湊率10％的運算能力，[這樣被駭的風險會在0.1％以下](https://en.bitcoin.it/wiki/Confirmation)。若是網路不暢通，則成熟與確認所需的次數都要增加！

![](.gitbook/assets/tu-2web.jpg)

## Merkle Tree；哈希樹



![](.gitbook/assets/image%20%283%29.png)





![](.gitbook/assets/image.png)





