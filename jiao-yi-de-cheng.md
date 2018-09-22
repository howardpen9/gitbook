# 交易的組成、結構

## Base58編碼 &  Base58Check 編碼 - 老鼠老虎

![&#x79C1;&#x9470;&#x3001;&#x516C;&#x9470;&#x3001;&#x9322;&#x5305;&#x5730;&#x5740;](.gitbook/assets/image%20%2820%29.png)



針對私鑰公鑰的推倒應該沒有什麼異議。主要問題是為何取得公鑰之後，要運算出收款地址\(Public Address\)時，在經由SHA256 & RIPEMD160運算後、還需要進行兩次公鑰的兩次SHA256哈希運算呢？

為了避免Public Address發生紀錄錯誤的情形。

![](.gitbook/assets/image%20%2822%29.png)

## BTC 腳本語言

比特幣網路中，交易的解析是有腳本\(Script\)性質的。是種簡單的單進程堆疊的概念去理解會簡單得多，交易的步驟如下圖所示。

![](.gitbook/assets/image%20%2821%29.png)







## 5 Types Standard Transactions

1. Pay-to-Public-Key-Hash
2. Pay-to-Public-Key
3. Multi-Signature \(MultiSig, 多重簽章交易\)
4. Data Output \(OP\_RETURN\) 可以填任意字串、具有保存文檔功能
5. Pay-to-Script-Hash 

### P2PKH \(Pay-to-Public-Key-Hash\) - 最常見的交易

[https://medium.com/@wilsonhuang/mastering-bitcoin-%E7%AD%86%E8%A8%98-standard-transactions-undone-bfb9b4ed0ed8](https://medium.com/@wilsonhuang/mastering-bitcoin-%E7%AD%86%E8%A8%98-standard-transactions-undone-bfb9b4ed0ed8)

{% embed data="{\"url\":\"https://blog.csdn.net/jerry81333/article/details/56824166\",\"type\":\"link\",\"title\":\"比特币 区块链 几种交易标准详解 P2PKH、P2PK、MS、P2SH加密方式 - CSDN博客\",\"description\":\"快速术语检索 以下文中会可能会用到，以及看此文前可能会需要理解的几个术语。 地址 比特币地址（例如：1DSrfJdB2AnWaFNgSbv3MZC2m74996JafV）由一串字符和数字组成，以阿拉伯数字“1”开头。就像别人向你的email地址发送电子邮件一样，他可以通过你的比特币地址向你发送比特币。  比特币 “比特币”既可以指这种虚拟货币单位，也指比特币网络或者网络节点使用的比特币\",\"icon\":{\"type\":\"icon\",\"url\":\"https://csdnimg.cn/public/favicon.ico\",\"aspectRatio\":0}}" %}

### P2PK \(Pay-to-Public-Key\)

P2PK鎖定版本的腳本形式如下：

```aspnet
<Public Key A> OP_CHECKSIG
```

用於解鎖的腳本是另一個簽名：

```aspnet
<Signature from Private Key A>
```

經由交易驗證軟件、確認的組合角本為：

```aspnet
<Signature from Private Key A> <Public Key A> OP_CHECKSIG
```





### Multi-Signature



### Data Output \(OP\_RETURN\)



### P2SH \(Pay-to-Script Hash\)

鎖定腳本

```aspnet
2 <Public Key A> <Public Key B> <Public Key C> 3 OP_CHECKMULTISIG
```

對上述的鎖定腳本進行SHA256 + RIPMED160 函式算法，形成20字節的腳本Hash:

```aspnet
8ac1d7a2fa204a16dc984fa81cfdf86a2a4e1731
```

於是乎鎖定腳本就變成為

```aspnet
OP_HASH160 8ac1d7a2fa204a16dc984fa81cfdf86a2a4e1731 OP_EQUAL
```

如此一來就可以縮短不必要的簽章以及Public Key位置。

解鎖腳本則是如前面一樣的結構：

```aspnet
<Sig1> <Sig2> <2 PK1 PK2 PK3 PK4 PK5 5 OP_CHECKMULTISIG>
```

最後將鎖定&解鎖腳本進行結合：

```aspnet
<Sig1> <Sig2> <2 PK1 PK2 PK3 PK4 PK5 5 OP_CHECKMULTISIG> OP_HASH160 8ac1d7a2fa204a16dc984fa81cfdf86a2a4e1731 OP_EQUAL
```











## BTC的文字敘述

![](.gitbook/assets/image%20%285%29.png)











