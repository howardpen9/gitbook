# 【繁體】比特幣中文版白皮書

來源網址:  [台北以太坊社群專欄](https://medium.com/taipei-ethereum-meetup/%E6%AF%94%E7%89%B9%E5%B9%A3-%E7%AB%AF%E5%B0%8D%E7%AB%AF%E9%9B%BB%E5%AD%90%E7%8F%BE%E9%87%91%E7%B3%BB%E7%B5%B1-bitcoin-a-peer-to-peer-electronic-cash-system-i-8a52de003c9)  
英文版白皮書: [https://bitcoin.org/bitcoin.pdf](https://bitcoin.org/bitcoin.pdf)

## 零、 摘要

> 完全以端對端（peer-to-peer）技術實現的電子現金系統允許線上支付可以不必透過金融機構而直接從某一方傳送。雖然[數位簽章（digital signature）](https://zh.wikipedia.org/wiki/%E6%95%B8%E4%BD%8D%E7%B0%BD%E7%AB%A0)解決了部分的問題，但若需要信任的第三方才能避免雙重支付（double-spending）的話，這個系統就沒有價值了。
>
> 我們提出了一個端對端的網路來解決**雙重支付**的問題，此網路透過以下方式對每一筆交易做[時間戳記（timestamp）](https://zh.wikipedia.org/wiki/%E6%99%82%E9%96%93%E6%88%B3)，也就是將每一筆交易[雜湊（hash）](https://zh.wikipedia.org/wiki/%E6%95%A3%E5%88%97)到一個「基於雜湊的工作量證明」（hash-based-proof-work）組成的一條不斷延伸的鏈（chain）上，除非重新完成所有工作量證明，這些紀錄將不能被更改。
>
> 最長的鏈不僅扮演了一連串目擊事件的證明，也證明了它來自於一個 CPU 運算能力很大的池。只要多數的 CPU 運算能力都由不會聯合攻擊網路的節點（node）所控制的話，誠實的節點就會產生一條最長且超越其他攻擊者的鏈。

> 這個網路本身需要的基礎建設很少，訊息會依據最大努力原則（best effort basis）**廣播**出去，而節點可自由選擇離開或重新加入網路、且**接受最長的工作量證明**所組成的**鏈**作為節點離開時所發生的交易事件之證明。

## ㄧ、介紹

網際網路的商業應用幾乎都仰賴金融機構作為處理電子支付之信任的第三方。雖然系統在大多數情況下都能運作良好，但這類系統仍有**以信任為基礎的模式**這項**固有的缺點**。

由於金融機構無法避免出面調解爭議，因此完全不可逆的交易並不真的可行。因為調解成本而提高的交易成本，將會限制最小的實際交易規糢、限制了日常小額支付、失去為不可逆之服務進行的不可逆支付的能力進而造成**更廣泛的成本**。**可逆性的服務讓信任的需求增加**，商人必須更加警惕自己的客戶，因此**會向他們索取完全不必要的個人資訊**。一定比例的詐騙可以被接受並認為是無法避免的。

以上這些成本及支付的不確定性在使用實體貨幣時皆可避免，但透過一個**缺乏第三方信任的溝通渠道**進行**支付**的機制並不存在。

我們所需要的是一個**以加密證明取代信任的電子支付系統**，且允許任何達成共識的雙方能夠直接交易，而不需信任的第三方參與。**計算不可逆**（Computationally impractical to reverse）的交易可以避免賣方受騙，而**常規性托管機制**（routine escrow mechanisms）則可以輕易地讓買方被保護。

在這篇論文中，我們透過一個P2P的**分散式時間戳記**伺服器去產生按時間排序的交易之計算證明，進而解決了雙重支付的問題。**只要誠實的節點能夠共同合作去控制比攻擊者族群更多的 CPU 運算能力，這個系統就會是安全的。**

## 二、交易（Transactions）

我們定義一顆電子貨幣為一串數位簽章。每位擁有者為前一個交易及下一位擁有者的[公開金鑰（public key）](https://zh.wikipedia.org/zh-tw/%E5%85%AC%E5%BC%80%E5%AF%86%E9%92%A5%E5%8A%A0%E5%AF%86)簽署一個雜湊的數位簽章，並將之加入一顆貨幣的尾端，透過上述的方式將貨幣發送給下一位擁有者。收款者可以透過檢查簽章（signature）來驗證該鏈的擁有者。

![&#x4EA4;&#x6613;&#xFF08;Transactions&#xFF09;](https://cdn-images-1.medium.com/max/800/1*jS8e6OsOo_qS7wHRqdfvaQ.png)

這個過程的問題在於**收款者無法驗證擁有者是否對這顆貨幣進行雙重支付**。一般解法為導入一個信任的中央權威機構或是造幣廠來驗證每一筆交易，以防止雙重支付。在每一筆交易之後，貨幣必須送回造幣廠以發行新貨幣，且只有直接從造幣廠發行的貨幣才會被信任為沒有雙重支付。然而，這個解法的問題在於整個金錢系統的命運皆仰賴在像銀行一樣會經手每筆交易的造幣廠公司。

我們需要一個方法讓收款者知道前一位擁有者並沒有簽署更早發生的交易，由於我們計較的是在此之前發生的交易，因此不用在意在此之後是否有雙重支付。證實交易存在的唯一方法是去意識到所有交易的發生，而在以造幣廠為基礎的糢式中，造幣廠必須意識到所有交易並決定交易完成的先後順序。

**為了在沒有信任的第三方的之情況下達到目的，交易必須被公告**，且我們需要一個讓所有參與者在他們接收的順序上達成共識的系統。收款者需要確保在交易期間絕大多數的節點都認同該交易是首次出現。

## 三、時間戳記伺服器（Timestamp Server）

**我們提出的解法先由時間戳記伺服器說起**。時間戳記伺服器會將由很多交易組成的區塊（block）所對應之雜湊**加上時間戳記**，並如同在報紙或 [Usenet](https://zh.wikipedia.org/wiki/Usenet)中發送的方法將雜湊進行廣播。時間戳記證明了資料在進入雜湊時必然存在。每個時間戳記應將前一個時間戳記加入其雜湊中，之後的每一個時間戳記都和前一個時間戳記進行增強\(reinforce\)，而形成了一條**鏈**。

![&#x6642;&#x9593;&#x6233;&#x8A18;&#x4F3A;&#x670D;&#x5668;&#xFF08;Timestamp Server&#xFF09;](https://cdn-images-1.medium.com/max/800/1*9ZVjRXzbPOAtEUT1fk_Ftg.png)

## 四、 工作量證明（Proof-of-Work）

為了在端對端的基礎上去實現一個分散式時間戳記伺服器，我們需使用一個類似 Adam Back 提出的 [Hashcash](http://www.hashcash.org/papers/hashcash.pdf) 技術的工作量證明系統，而非像報紙或 Usenet 的發送方法。工作量證明包括掃描一個數值，以 [SHA-256](http://programmermagazine.github.io/201401/htm/message2.html) 安全雜湊演算法（Secure Hash Algorithm）為例，雜湊值以一個或多個 0 開始。

平均而言，**隨著 0 的數目上升、所需之工作量將呈指數增加**，且可藉由執行一次雜湊運算即完成驗證。

在我們的時間戳記網路中，**我們藉由增加區塊中 nonce 的數量**直到提供區塊所對應之雜湊足夠數量的 0 之 nounce 值被發現來完成工作量證明。一旦 CPU 的貢獻擴大到足以滿足工作量證明，除非重新完成一定的工作量否則該區塊無法被更改。當後來的區塊在此之後被鏈結上，**若想更改區塊，則需要完成之後所有區塊的工作量。**

![&#x5DE5;&#x4F5C;&#x91CF;&#x8B49;&#x660E;&#xFF08;Proof of Work&#xFF09;](https://cdn-images-1.medium.com/max/800/1*rh6Y2EScZjST7Ojqs_dxWw.png)

工作量證明也解決了多數決代表的問題。如果多數是建立在一個 IP 位址對應一票的基礎上，則會被任何可以擁有許多 IP 者所推翻。工作量證明本質上是一個 CPU 對應一票。**多數決代表為最長的鏈，因為最長的鏈包含了最大的工作量。**如果大多數的 CPU 運算能力是由誠實的節點所控制，則此誠實的鏈將會成長為最快速且超越任何一條競爭者的鏈。

若要修改先前的區塊，攻擊者必須重新完成該區塊及在它之後的所有區塊的工作量證明，並趕上且超越誠實節點之工作量。我們稍後會說明較慢的攻擊者趕上的機率會隨著之後的區塊增加而呈指數消減。**`[註1]`**

為了解決因硬體速度的增加及節點參與網路的程度起伏之問題，工作量證明的難度是由每小時平均產生之區塊數量而決定。**如果區塊答案越快被算出、則難度也會隨之增加。**

## 五、網路（Network）

以下是網路運作的步驟：

1. 新的交易會**向所有節點廣播**
2. 每個節點會**蒐集**加入區塊的**新交易**
3. 每個節點負責為其區塊找到一條夠困難的工作量證明
4. 當節點**找到工作量證明後**，會**向所有節點廣播**
5. **節點只有在節點中的所有交易為有效且尚未存在過時，才會接受區塊為有效的**
6. 節點會使用已接納之區塊的雜湊作為之前的雜湊來創造鏈上的新區塊，來表示接受該區塊

**節點始終會將最長的鏈視為正確的**且持續延長。若有兩個節點同時廣播兩個不同版本的新區塊，部分節點可能會先接收到其中一種。在這種情況下，節點會先處理先接收到的區塊，但也會保存另一條分支鏈以免它變成最長的鏈。當下一個工作量證明被發現且其中一條鏈被證實為較長的一條時，這樣的僵局會被打破，此時，原本在另一條分支鏈上工作的節點會轉而處理較長的這條鏈。

新交易的廣播並不一定要觸及所有節點，只要這些交易廣播到足夠多的節點，不久後這些交易就會被整合進一個區塊。區塊的廣播同樣可以容許漏掉訊息，若某個節點沒有接收到特定區塊，當這個節點接收到下一個區塊時，就會認知到它遺漏了一個區塊並可提出自己下載該區塊的請求。

## 六、 獎勵（Incentive）

按照慣例，區塊中的第一個交易較特別，因為它產生了一顆由該區塊建立者所擁有的新貨幣。

這**激勵了節點\(礦工\)去支持整個網路**，且在沒有中央權威機構發行貨幣的情況下，提供了初期貨幣流通的方法。新貨幣數量的穩定增加和礦工為了增加黃金流通而耗費資源去挖礦類似。

而在我們的案例中，耗費的資源是 **CPU 運算時間**和**消耗的電力**。

獎勵也可以來自於**交易手續費**。_**交易的輸入值減掉輸出值即為交易的手續費**_，提供交易所在之區塊的獎勵。一旦既定數量的貨幣進入流通，獎勵機制就可以完全依靠交易手續費且能免於通貨膨脹。

獎勵機制也有助於鼓勵節點保持誠實。若有貪婪的攻擊者能夠裝備比所有誠實節點還要多的 CPU 運算能力，那麼他將面臨以下選擇：**進行雙重支付的攻擊**或是**用於誠實工作來產生新貨幣**。

他將會發現遵循規則將是更**有利可圖**的，因為比起破壞系統及自身財產的合法性，**遵守規則**能夠使他擁有更多的電子貨幣。

## 七、 回收磁碟空間（Reclaiming Disk Space）

當一個貨幣的最新交易被整合進足夠數量的區塊中，則可丟棄在此交易之前的資料以節省磁碟空間。為了不破壞區塊的 hash，交易資訊會被 hash 至一顆 [Merkle Tree](http://www.itread01.com/articles/1487247623.html) 中，且只有根包含在區塊 hash 中。舊的區塊接著可藉由去除樹的分支來壓縮，內部的 hash 也不需要被保存。

![&#x56DE;&#x6536;&#x78C1;&#x789F;&#x5340;&#xFF08;Reclaiming Disk Space&#xFF09;](https://cdn-images-1.medium.com/max/800/1*oLgRVR3joPISJdXHF9OR4w.png)

不含交易資訊的區塊標頭（header）約為 80 bytes，假設每十分鐘產生一個區塊，則一年需要 80 bytes \* 6 \* 24 \* 365 = 4.2 MB。由於 2008 年販售的電腦系統搭載 2GB 的 RAM，且 Moore’s law 預測每年可成長 1.2 GB，因此，即便區塊標頭必須被保存在記憶體中也不會是問題。

## 八、簡化的支付驗證（Simplified Payment Verification）

**不運行整個網路的節點\(≒170GB\)來驗證支付是可行的**，使用者只需要保存最長的工作量證明鏈的區塊標頭檔案即可，此備份可藉由不斷詢問網路節點直到它被證可為最長鏈來取得，且透過 Merkle Tree 的分支鏈結上被加上時間戳記且被整合進區塊的交易。

**雖然使用者本來就無法自行驗證交易的有效性**，但藉由跟全網的其他帳本進行連結後、使用者就可以看到網路上的節點是否接受過這筆交易紀錄，且**藉由之後加上的區塊愈多、你更可以進一步證明了整個網路承認這筆交易的存在性與真實性**。

![&#x7C21;&#x5316;&#x7684;&#x652F;&#x4ED8;&#x9A57;&#x8B49;&#xFF08;Simplified Payment Verification&#xFF09;](https://cdn-images-1.medium.com/max/800/1*0m4XX-g3DHC9leN32cbzDg.png)

因此，只要誠實的節點控制了網路，驗證機制就會是可靠的。但若**攻擊者擁有過多運算能力**的話，驗證機制就會**變得較脆弱**。**因為網路節點可以自己驗證交易**，一旦攻擊者持續掌控了整個網路，這樣簡易的驗證方法將會被攻擊者所產生的交易欺騙。

為了避免這樣的情況發生，當網路節點偵測到無效的區塊時就會發出警報，並提示使用者的軟體去下載整個區塊以及被警告有問題的交易來對資訊的前後不一致做判定。

對於頻繁接收支付的商業機構而言，可能會為了更獨立的安全性及更快速的驗證而希望能夠運行自己的節點。

## 九、 幣值的結合與分割（Combining and Splitting Value）

雖然可以個別處理貨幣，但要為每次轉帳時每一顆\(分\)貨幣都建立一個單獨的交易是很麻煩的。為了讓幣值得以結合及分割，交易將會有許多輸入與輸出。通常會有前一個交易共同組成的**多重輸入**，且輸出最多只會有**兩個**：一個為**支付的費用**，另一個為**必要時的找零**。

![&#x5E63;&#x503C;&#x7684;&#x7D50;&#x5408;&#x8207;&#x5206;&#x5272;&#xFF08;Combining and Splitting Value&#xFF09;](https://cdn-images-1.medium.com/max/800/1*4hLF4AlS1uK0CVDxDRBMjg.png)

值得注意的是，**一個交易依賴很多筆交易**；而那些交易又依賴更多筆交易，但這不會是一個問題，因為這個運作並不需要去**展開並驗證**一個交易的歷史資料。

## 十、隱私（Privacy）

傳統銀行的模式是藉由限制交易的參與者及信任的第三方取得資訊的權限來達到一定程度的隱私，但若公開發佈所有交易就牴觸了上述的方法，但仍可藉由保持公開金鑰的匿名性來維持隱私。大眾可以看到某人傳了一筆金額給另一人，但卻無法得知這筆交易對應到誰。

#### 這和股票交易所釋出的交易資訊是同樣等級的，也就是可以知道交易發生的時間與交易量，但無法得知交易的兩方是誰。

![&#x96B1;&#x79C1;&#xFF08;Privacy&#xFF09;](https://cdn-images-1.medium.com/max/800/1*vdbe2UvElK7Szi44_YX1rg.png)

作為額外的預防措施，一對新的金鑰應該讓每筆交易避免被鏈結到同一個擁有者。有些鏈結仍不免為多重輸入的交易，這也代表這些輸入被同一人所擁有，這樣的風險在於一旦擁有者的金鑰被揭露，這些鏈結可能也會同時揭露了來自這個擁有者的其他交易。

## 十一、計算（Calculations）\[註2\]

即便一個攻擊者比誠實鏈更快速產生一條替代的鏈，也無法讓系統變成可任意變更，即無法憑空創造幣值或取得不屬於攻擊者的貨幣。節點不會接受無效的支付交易，所以誠實的節點永遠不會接受包含無效交易的區塊。攻擊者只能試著改變其中一筆最近的交易來取回剛剛支付的貨幣。

可以把誠實鏈和攻擊者的鏈之間的競爭視為[二項隨機漫步（Binomial Random Walk）](https://math.stackexchange.com/questions/1766678/binomial-random-walk)，定義成功事件為誠實鏈延長了一個區塊，使其領先性加 1；失敗事件則是攻擊者鏈被延長了一個區塊，使得差距減 1。

攻擊者由落後迎頭趕上誠實鏈的機率和[賭徒破產問題（Gambler’s Ruin problem）](https://zh.wikipedia.org/wiki/%E8%B5%8C%E5%BE%92%E7%A0%B4%E4%BA%A7%E7%90%86%E8%AE%BA)類似，假設一個擁有無限籌碼的賭徒一開始為赤字狀態，但可能在經過無限次的嘗試後達到收支平衡。我們可以計算出攻擊者收支平衡或趕上誠實鏈的機率：

![](https://cdn-images-1.medium.com/max/800/1*yC5_kk2Ij60a8b-YNVJTwQ.png)

假設 p &gt; q，則攻擊成功的機率將會隨著攻擊者需要趕上的區塊數量增加而呈指數下降。因為攻擊者的勝算不高，因此，若他沒能幸運的快速成功，則他獲得成功的機會就會隨時間流逝而變得更加微乎其微。

我們現在考慮的是，在一個新交易中充分確定發送者無法竄改交易之前，接收者必須等待多久。我們假設發送者是攻擊者，它讓接收者在一段期間內相信它已經支付款項，接著立刻將支付的款項重新支付給自己。當這個狀況發生時，接收者會收到警報，但攻擊者會希望這個警報為時已晚。

接收者會產生一對新的金鑰，並在簽章之前會立即將公開金鑰傳給發送者，如此能避免以下情況發生：發送者預先準備一條區塊鏈然後持續對此區塊進行運算，一直到幸運地讓這條區塊鏈領先了誠實鏈，才立即執行支付。一旦交易被發送出去，不誠實的發送者就會開始秘密地準備包含該交易替代版本的平行鏈。

接收者會等待直到交易被加到首個區塊上且在那之後鏈結上 z 個區塊，此時接收者仍無法得知攻擊者實際的進展，但若假設每個誠實區塊以平均的預定時間產生，則攻擊者可能的進展會是一個 Poisson 分佈（Poisson Distribution），分布的期望值為：λ = z \* \(q/p\)。

為了得到攻擊者現階段可以趕上的機率，我們將攻擊者在該數量下依然能夠趕上的機率乘上每個攻擊者可以產生的進展量之 Poisson 機率密度：

![](https://cdn-images-1.medium.com/max/800/1*HkNSyCXjNp74guTnqxwPKA.png)

為了避免對無窮數列求和而將式子修改成：

![](https://cdn-images-1.medium.com/max/800/1*9YNgav_yQikcQ5QwBOZSRg.png)

以 C 語言去實現：

![](https://cdn-images-1.medium.com/max/800/1*9tC4g7TzBSSKOoaG_HjxQA.png)

可以從運算結果發現機率會隨著 z 值變大而呈指數下降：

![](https://cdn-images-1.medium.com/max/800/1*hoKp6q2wsJS6TWjuQzJfuQ.png)

以 P 值小於 0.1% 去求解 z 值：

![](https://cdn-images-1.medium.com/max/800/1*DODFPY3JICXmu-vKCx4GSw.png)

## 十二、結論

 我們提出了一個**無須仰賴信任的電子交易系統**。我們首先討論了電子貨幣的數位簽章原理，它提供了擁有者很大的控制力，但仍不足以防止雙重支付（double-spending）的發生。為了解決這個問題，我們提出了一個採用工作量證明（proof-of-work）機制的端對端網路來記錄交易的公開歷史資訊，若誠實的節點掌控了 CPU 運算能力，則攻擊者去竄改交易資訊是計算上不可行的。

這個網路的強健之處在於其結構上的簡潔性。節點們不太需要彼此協調就能同時執行工作。由於訊息不會被傳送到任何特定的地方，因此節點們不需要被識別，只需要以最大努力原則被傳送。節點可以自由選擇離開或重新加入網路，且會接受工作量證明鏈為當節點不在網路時所發生的交易事件之證明。節點以各自的 CPU 運算能力來進行投票，表決對有效區塊的驗證，以不斷延長有效的區塊鏈來表示接受，而以拒絕在無效的區塊之後延長來表示拒絕。這個**共識機制\(consensus mechanism\)**包含了一個端對端的電子貨幣系統所需要的所有規則及獎勵機制。

#### \[參考資料\] {#ecfa}

1. W. Dai, “b-money,” [http://www.weidai.com/bmoney.txt](http://www.weidai.com/bmoney.txt), 1998.
2. H. Massias, X.S. Avila, and J.-J. Quisquater, “Design of a secure timestamping service with minimal trust requirements,” In _20th Symposium on Information Theory in the Benelux_, May 1999.
3. S. Haber, W.S. Stornetta, “How to time-stamp a digital document,” In _Journal of Cryptology_, vol 3, no 2, pages 99–111, 1991.
4. D. Bayer, S. Haber, W.S. Stornetta, “Improving the efficiency and reliability of digital time-stamping,” In _Sequences II: Methods in Communication, Security and Computer Science_, pages 329–334, 1993.
5. S. Haber, W.S. Stornetta, “Secure names for bit-strings,” In _Proceedings of the 4th ACM Conference_ _on Computer and Communications Security_, pages 28–35, April 1997.
6. A. Back, “Hashcash — a denial of service counter-measure,” [http://www.hashcash.org/papers/hashcash.pdf](http://www.hashcash.org/papers/hashcash.pdf), 2002.
7. R.C. Merkle, “Protocols for public key cryptosystems,” In _Proc. 1980 Symposium on Security and_ _Privacy_, IEEE Computer Society, pages 122–133, April 1980.
8. W. Feller, “An introduction to probability theory and its applications,” 1957.

\[註1\] 作弊造假者、誠實節點、51%攻擊  
\[註2\]對於中本聰這篇論文中提到的攻擊者與誠實鏈之間的競爭分析，此篇[論文](https://arxiv.org/abs/1702.02867)提出了修正與更新。
