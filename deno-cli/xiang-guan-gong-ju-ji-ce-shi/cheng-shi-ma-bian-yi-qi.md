---
description: Deno 1.6 所新增的功能，可以讓開發者編譯出可執行的二進制檔案。
---

# 程式碼編譯器

### 前置作業

在開始之前，請確保你的 Deno 版本為 `1.6.0` 以上。

![&#x4F7F;&#x7528; deno upgrade &#x9032;&#x884C;&#x66F4;&#x65B0;&#x3002;](../../.gitbook/assets/image.png)

### 進入正題

更新到最新版本後，我們一樣將畫面停留在命令列並輸入:

```text
deno compile --unstable https://deno.land/std@0.79.0/http/file_server.ts
```

這時，只要靜靜等待編譯完即可。

編譯完成後，我們會看到如下圖右方的訊息:

```text
// ...
Emit file_server
```

接著，我們可以執行該檔案，便會順利看到下圖左方的畫面:

```text
./file_server.exe
```

![](../../.gitbook/assets/image%20%281%29.png)



