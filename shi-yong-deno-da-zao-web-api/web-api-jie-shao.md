---
description: >-
  終於來到本系列文的最終階段 - Web API 設計與實作，這部分會講的非常細，包含 Web API 設計理念、設計、實作，並且學習使用 Oak,
  Denon, denomongo, mongoDB, postman 實作出並測試屬於自己的第一個 Deno Web API。
---

# Web API 介紹

### 進入正題

Web API ，顧名思義就是藉由 Web 服務提供的軟體外部接口。更精確一點的說法則是**使用 HTTP 協定通過網路調用 API** 。

以本系列會做出的作品為例，訪問以下 URL:

```text
http://127.0.0.1:3001/user/Ian
```

便可以得到 Web API 回傳的資料:

```text
{"_id":{"$oid":"5f50d090f7e761bf69556865"},"name":"Ian"}
```

只要你願意，也可以做出一個加減乘除的 API ，不過這個 API 就沒有太多實用性了。

#### Web API 的重要性

現今資訊屆龍頭之一的 `Amazon` 早在 10 多年前就開始提供了 Web API ，在當時便很大的影響互聯網世界。

為什麼呢? 因為商家能夠透過該 API 將 Amazon 上的商品內嵌到自己的網站進行銷售，進而向 Amazon 分取利潤。

這樣的商業模式，也讓 Amazon 的收益快速成長，更在日後提供了 EC2, S3...等服務。

#### API 的多樣性

如果讀者是一位 Modern Web 的居民，對於前後端分離的觀念肯定不陌生。前後端分離的概念被越來越多的企業、開發者採用，這也意味著: 多數網頁服務的背後都是有一個或多個 Web API 支撐起來的。也因為這樣，讓 Web API 被應用在多種情況:

* 已發布的 Web 服務透過 API 公開

  以國內知名的迷片網站 Avgle 為例，該網站本身是以網頁形式提供服務的，不過它其實也有開放 Web API 讓開發者能夠拿來料理成屬於自己的 APP 。

  > 關鍵字大概就是: Avgle, Web API XDDDD

* 插件功能

  以 CodePen 為例，它提供了內嵌式的插件讓開發者能在自己的網站上嵌入他人分享在 CodePen 上的程式碼/編譯結果。

  或是 Youtube 同樣也有提供 API 讓開發者可以在網站上嵌入迷你的播放器。

* 用於製作 SPA \(Single Page Application\)

  有越來越多的網站以 SPA 的形式被開發出來，要開發 SPA 就必須利用前後端分離的概念實作，因此也會廣泛的使用到 Web API 。

* 整合多項系統

### 問與答: Web API 與 Restful API 的差異

常常聽到大家說的 **Restful API** ，至於為何在本系列是強調 **Web API** 入門呢?

其實 Restful API 就是遵照 [Representational state transfer](https://en.m.wikipedia.org/wiki/Representational_state_transfer) 所實作的 Web API ，本系列文目前沒有這個打算。因此，才會強調是 Web API 的介紹唷!

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [Web API 是否一定要 RESTful?](https://blog.darkthread.net/blog/is-restful-required/)

