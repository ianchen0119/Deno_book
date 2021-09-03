---
description: 'Deno 1.7 中新增了許多功能，其中還多了一個全新的 API: Deno.resolveDns 。'
---

# Deno.resolveDns

### What is DNS?

請參考[什麼是 DNS？ \| DNS 的工作方式](https://www.cloudflare.com/zh-tw/learning/dns/what-is-dns/)。

### Deno.resolveDns

了解什麼是 DNS 後，一起來看看這個全新的 API 要如何使用吧!

Deno.resolveDns 可以讓開發者查詢 DNS 紀錄，參考以下程式碼:

```text
const domainName = Deno.args[0];
if (!domainName) {
  throw new Error("Domain name not specified in first argument");
}

const records = await Deno.resolveDns(domainName, "A");
for (const ip of records) {
  console.log(ip);
}
```

執行結果:

```text
$ deno run --allow-net --unstable https://deno.land/posts/v1.7/dig.ts deno.land
104.21.18.123
172.67.181.211
```

