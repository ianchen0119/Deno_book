# Deno 與 MongoDB 共舞

### 開始之前： 設定 collection 以及 database

繼昨天的安裝教學，我們現在來建立待會要使用的 database 以及 collection ：

1. 打開 MongoDB Compass 並按下 `Create Database` 。

![https://ithelp.ithome.com.tw/upload/images/20201019/20110850Ld4emQn82M.png](https://ithelp.ithome.com.tw/upload/images/20201019/20110850Ld4emQn82M.png)

1. 輸入 Database 和 Collection 的名字：

![https://ithelp.ithome.com.tw/upload/images/20201019/20110850kypRBuvnW1.png](https://ithelp.ithome.com.tw/upload/images/20201019/20110850kypRBuvnW1.png)

1. 創建完成，如果讀者有需要，也可以先插幾筆資料進去。

![https://ithelp.ithome.com.tw/upload/images/20201019/201108504Eab6kIbd1.png](https://ithelp.ithome.com.tw/upload/images/20201019/201108504Eab6kIbd1.png)

### 進入正題： deno\_mongo 教學

`deno_mongo` 是一款 MongoDB 資料庫驅動的 Deno 第三方套件，它是基於 rust 官方的 mongoldb Library 所打造的。

#### 建立連線

在開始之前，需要將第三方模組引入：

```text
import { MongoClient } from "https://deno.land/x/mongo@v0.13.0/mod.ts";
```

引入後，將 `MongoClient` 物件實例化：

```text
const client = new MongoClient();
```

開啟 MongoDB 後，就可以使用 `client` 建立連線：

```text
client.connectWithUri("mongodb://localhost:27017");
```

執行到這一步時，就已經成功連上 MongoDB 了，不過我們還沒告訴 Deno 要存取資料庫中的哪一個 database 和 collection ：

```text
const db = client.database("test");
const users = db.collection("users");
```

上面的範例是讓 Deno 連上 MongoDB 中 `test` database 的 `users` collection 。

#### 使用 Interface 防止非預期資料寫入 MongoDB

使用 Interface 可以有效預防有心人士在 json 格式內插入不需要的資料。

假設我們預期 collection 的欄位只有 `id` 、 `username` 以及 `password` ，可以先定義好 interface ：

```text
interface UserSchema {
  _id: { $oid: string };
  username: string;
  password: string;
}
```

定義好後，我們在連結 collection 的那行程式碼加入 interface 檢查：

```text
const db = client.database("test");
const users = db.collection("users");
```

### 常見操作

在學會如何與 MongoDB 建立連線後，跟著筆者一起看看 deno\_mongo 提供的操作方法吧！

> 注意！進行以下常見操作都需要先將 Deno 與 MongoDB 做連接唷！
>
> ```text
> import { MongoClient } from "https://deno.land/x/mongo@v0.13.0/mod.ts";
> const client = new MongoClient();
> client.connectWithUri("mongodb://localhost:27017");
> const db = client.database("test");
> const users = db.collection("users");
> ```

#### 新增單筆資料

```text
const insertId = await users.insertOne({
  username: "user1",
  password: "pass1",
});
```

#### 新增多筆資料

```text
const insertIds = await users.insertMany([
  {
    username: "user1",
    password: "pass1",
  },
  {
    username: "user2",
    password: "pass2",
  },
]);
```

#### 尋找單筆資料

```text
const user1 = await users.findOne({ _id: insertId });
```

#### 尋找多筆資料

```text
const all_users = await users.find({ username: { $ne: null } });
```

#### 計數

計算 Collection 中有多少筆資料包含使用者輸入的鍵值對 ：

```text
const count = await users.count({ username: { $ne: null } });
```

#### 聚合

聚合操作主要用於處理數據 \(像是：取平均值、加總等操作\)並回傳計算後的結果。

```text
const docs = await users.aggregate([
  { $match: { username: "many" } },
  { $group: { _id: "$username", total: { $sum: 1 } } },
]);
```

#### 新增單筆資料

```text
const { matchedCount, modifiedCount, upsertedId } = await users.updateOne(
  { username: { $ne: null } },
  { $set: { username: "USERNAME" } }
);
```

#### 新增多筆資料

```text
const { matchedCount, modifiedCount, upsertedId } = await users.updateMany(
  { username: { $ne: null } },
  { $set: { username: "USERNAME" } }
);
```

#### 刪除單筆資料

```text
const deleteCount = await users.deleteOne({ _id: insertId });
```

#### 刪除多筆資料

```text
const deleteCount2 = await users.deleteMany({ username: "test" });
```

#### 跳過

我們可以使用 `skip()` 跳過指定數量的資料：

```text
const skipTwo = await users.skip(2).find();
```

#### 限制

如果使用者需要在 MongoDB 中找尋數據並指定數據記錄的數量，可以使用 `limit()` ：

```text
const featuredUser = await users.limit(5).find();
```

### 執行

最後要注意的是， `deno_mongo` 目前還不穩定，使用上不要忘記加入 `--unstable` 標籤唷！

```text
deno run --allow-net --allow-write --allow-read --allow-plugin --allow-env --unstable yourCode.ts
```

### 總結

本篇文章提到了如何使用第三方套件去連線 MongoDB 。明天筆者就會將簡易 WebAPI 留下的坑填完，順便分享一下什麼是 ORM ，我們明天見！

> 此外，本篇所使用的範例程式碼參考了 [deno\_mongo](https://deno.land/x/mongo@v0.13.0) 的官方文件，請知悉。

