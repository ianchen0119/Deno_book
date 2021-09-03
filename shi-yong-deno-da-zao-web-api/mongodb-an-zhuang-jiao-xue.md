# MongoDB 安裝教學

### 開始之前: 什麼是 MongoDB ?

MongoDB 是眾多 NoSQL 中最受歡迎的資料庫之一。它能儲存 JSON 及 Schema-free 的資料，對比傳統的關聯式資料庫 \(MySQL 等等\)， MongoDB 對於巨量資料、高併發以及高可靠性有更為強大的支援。

此外，比起傳統的關聯式資料庫， MongoDB \(也幾乎是 NoSQL 的優點\)有如下優點:

1. 不需要事先定義資料文件的結構與資料之間的關聯
2. 也因為不需定義結構，使用者可以自由的新增欄位，不需回頭修改過去的資料文件
3. 能在不停機的狀態下，水平式的擴展和資料自動平衡

#### The History of NoSQL

> NOSQL 一詞最早出現於1998年，是 Carlo Strozzi 開發的一個輕量、開源、不提供 SQL 功能的關聯式資料庫\[[1\]](https://zh.wikipedia.org/wiki/NoSQL#cite_note-1)。
>
> 2009年， Last.fm 的 Johan Oskarsson 發起了一次關於分散式開源資料庫的討論\[[2\]](https://zh.wikipedia.org/wiki/NoSQL#cite_note-2)，來自 Rackspace 的 Eric Evans 再次提出了NOSQL 的概念，這時的 NOSQL 主要指非關係型、分散式、不提供 [ACID](https://zh.wikipedia.org/wiki/ACID) 的資料庫設計模式。
>
> 2009年在亞特蘭大舉行的 "no:sql\(east\)" 討論會是一個里程碑，其口號是 "select fun, profit from real\_world where relational=false;" 。因此，對 NOSQL 最普遍的解釋是「非關聯型的」，強調[鍵-值儲存](https://zh.wikipedia.org/wiki/%E9%94%AE-%E5%80%BC%E5%AD%98%E5%82%A8)和[文件導向資料庫](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E6%96%87%E6%A1%A3%E6%95%B0%E6%8D%AE%E5%BA%93)的優點，而不是單純的反對 RDBMS 。
>
> 基於2014年的收入， NOSQL 市場領先企業是 [MarkLogic](https://zh.wikipedia.org/w/index.php?title=MarkLogic&action=edit&redlink=1) ，[MongoDB](https://zh.wikipedia.org/wiki/MongoDB)和[Datastax](https://zh.wikipedia.org/w/index.php?title=Datastax&action=edit&redlink=1)\[[3\]](https://zh.wikipedia.org/wiki/NoSQL#cite_note-3)。基於2015年的人氣排名，最受歡迎的NOSQL資料庫是 [MongoDB](https://zh.wikipedia.org/wiki/MongoDB) ， [Apache Cassandra](https://zh.wikipedia.org/wiki/Apache_Cassandra) 和 [Redis](https://zh.wikipedia.org/wiki/Redis)\[[4\]](https://zh.wikipedia.org/wiki/NoSQL#cite_note-4) 。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/NoSQL)

#### 為何選擇 MongoDB

因為在 MongoDB 中，資料的儲存架構是以 JSON 格式儲存。我們在 Oak 程式從 MongoDB 上取得資料後，就能直接對 JSON 格式的資料進行操作。

### 進入正題

首先，我們先到 [MongoDB 的官網](https://www.mongodb.com/try/download/community)取得安裝檔:

![https://ithelp.ithome.com.tw/upload/images/20201018/20110850J9FXCy7E86.png](https://ithelp.ithome.com.tw/upload/images/20201018/20110850J9FXCy7E86.png)

下載安裝檔後，直接打開安裝檔準備進行安裝:

![https://ithelp.ithome.com.tw/upload/images/20201018/20110850YP6hezNAxn.png](https://ithelp.ithome.com.tw/upload/images/20201018/20110850YP6hezNAxn.png)

打開安裝檔後，會看見引導程式，直接按下一步 \(Next\)。

![https://ithelp.ithome.com.tw/upload/images/20201018/20110850BjdMnjAIo3.png](https://ithelp.ithome.com.tw/upload/images/20201018/20110850BjdMnjAIo3.png)

接下來會看到聲明合約，當然同意，哪次不同意?

![https://ithelp.ithome.com.tw/upload/images/20201018/20110850yy9b6DzaGH.png](https://ithelp.ithome.com.tw/upload/images/20201018/20110850yy9b6DzaGH.png)

因為筆者沒有特殊要求，就直接選擇 Complete 將安裝程序完成就好。

![https://ithelp.ithome.com.tw/upload/images/20201018/20110850hjIKfca5PR.png](https://ithelp.ithome.com.tw/upload/images/20201018/20110850hjIKfca5PR.png)

來到服務確認頁面，筆者這邊是原封不動的直接按下一步。

![https://ithelp.ithome.com.tw/upload/images/20201018/201108509OIgcssYri.png](https://ithelp.ithome.com.tw/upload/images/20201018/201108509OIgcssYri.png)

MongoDB Compass 是官方提供的圖形化介面，因為方便入門，所以筆者選擇安裝並進行下一步。

![https://ithelp.ithome.com.tw/upload/images/20201018/20110850ZZNmhr4X4b.png](https://ithelp.ithome.com.tw/upload/images/20201018/20110850ZZNmhr4X4b.png)

安裝完成後， MongoDB Compass 就會自動啟動拉 !

> 今天筆者只會介紹 MongoDB 的安裝部分，至於如何設定 Collection 等等，筆者會將它與 Oak 串接教學放到同一篇文章一起說明。

### 題外話

如果沒有意外的話，筆者明天也會繼續產出文章。昨天出門大吃大喝一整天，回來的時候已經11點多所以才沒有發文，真的十分抱歉 QQ

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [學習手記：2018清華大學DB/AI Bootcamp — II — B-Tree Indexing](https://medium.com/hallblazzar-%E9%96%8B%E7%99%BC%E8%80%85%E6%97%A5%E8%AA%8C/%E5%AD%B8%E7%BF%92%E6%89%8B%E8%A8%98-2018%E6%B8%85%E8%8F%AF%E5%A4%A7%E5%AD%B8db-ai-bootcamp-ii-b-tree-indexing-648a096e1598)

  > MongoDB 的底層正是 B-Tree ，如果對於資料庫底層是如何操作有興趣，大家可以參考該篇文章。

* [從入門到精通 MongoDB](https://ithelp.ithome.com.tw/users/20130448/ironman/3618)

  > 幫友 Andylinee 的系列文，該系列從 MongoDB 的安裝、介紹再到基本的 CRUD 操作...應有盡有，如果讀者接資料庫接出興趣，也可以參考本系列文。

 [![](https://ithelp.ithome.com.tw/upload/images/banners/sjzFcuxo.gif)](https://ithelp.ithome.com.tw/api/banners/sjzFcuxo/click)

