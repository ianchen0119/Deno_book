# 使用 Denon 精簡指令

筆者在昨天非常粗略地介紹了 Oak 的概念。此外，在開始開發 Web API 之前，筆者想先介紹一款好用的工具給各位讀者。

因為 Deno 引入了沙盒的安全設計理念，我們在執行程式的時候都需要考量程式所需的環境在執行命令加上各種標籤，像是: `--unstable` 、 `--allow-net` 等等。竟然我們都知道自己開發的程式需要哪些環境，真的只能選擇在每次執行時都敲這麼多攏長的標籤嗎?

不 ! [Denon](https://deno.land/x/denon@2.4.4) 的出現，幫助我們解決掉這個惱人的問題。

### 進入正題

![Denon](https://cdn.deno.land/denon/versions/2.4.4/raw/assets/denon-horizontal.svg)

Denon 之於 Deno ，就好比 Nodemon 之於 Node.js 。

Denon 並不會對使用者的程式碼或開發方法產生任何變化， 其最主要的用途就是可以讓使用者對 Deno 指令做包裝。

#### 功能

Denon 的主要特點以及功能如下:

* 當程式碼改變，可以自動重啟你的 Deno 程式
* 直接替換 deno 可執行文件
* 支持腳本的擴充選項
* 可配置文件監視程序，支持文件系統事件和目錄遍歷
* 忽略 [glob](https://en.wikipedia.org/wiki/Glob_%28programming%29) 模式的特定文件或目錄

#### 安裝

安裝方法很簡單，只要使用以下命令即可完成:

```text
deno install -qAf --unstable https://deno.land/x/denon@2.4.4/denon.ts
```

> 需要注意的是 : 請確認讀者使用的 Deno 版本在 `1.4.0` 以上。

#### 文件配置

Denon 可以根據使用者的需求進行配置。它支持 json 和 yaml 作為配置文件的格式。

如果要在個人專案使用 Denon ，可以使用該命令去創建一個配置文件 `scripts.json` ：

```text
{
  "scripts": {
    "start": "app.js"
  }
}
```

此外，我們也可以利用 TypeScript 去設定 Denon 的配置:

```text
denon --init typescript
```

> 文件格式請參考 [src / templates.ts](https://github.com/denosaurs/denon/tree/master/src/templates.ts) 。

**JSON 樣板**

```text
{
  // optional but highly recommended
  "$schema": "https://deno.land/x/denon/schema.json",

  "scripts": {
    "start": {
      "cmd": "deno run app.ts",
      "desc": "run my app.ts file"
    }
  }
}
```

**YAML 樣板**

```text
scripts:
  start:
    cmd: "deno run app.ts"
    desc: "run my app.ts file"
```

#### 腳本的可用選項

* 環境變數

  我們可以將環境變數傳遞給子程序 \(這裡指 `app.ts` \)。

  ```text
  {
    // globally applied to all scripts
    "env": {
      "TOKEN": "SUPER SECRET TOKEN"
    },

    "scripts": {
      "start": {
        "cmd": "deno run app.ts",
        "desc": "Run the main server.",
        "env": {
          "PORT": 3000
        }
      }
    }
  }
  ```

* 權限

  設定 `app.ts` 的權限:

  ```text
  {
    // globally applied to all scripts
    // as object ...
    "allow": {
      "read": "/etc,/tmp", // --allow-read=/etc,/tmp
      "env": true     // --allow-env
    },
    // ... or as array
    "allow": [
      "run", // --allow-run
      "net" // --allow-net
    ]

    "scripts": {
      "start": {
        "cmd": "deno run app.ts",
        "desc": "Run the main server.",

        // specific for a single script
        // as object ...
        "allow": {
          "read": "/etc,/tmp", // --allow-read=/etc,/tmp
          "env": true     // --allow-env
        },
        // ... or as array
        "allow": [
          "run", // --allow-run
          "net" // --allow-net
        ]
      }
    }
  }
  ```

  除了上述做法，其實也可以直接把標籤加入 `cmd` 中:

  ```text
  {
    "scripts": {
      "start": "deno run -A --unstable app.ts"
    }
  }
  ```

* 關閉文件監控

  前面有提到: 當程式碼改變，可以自動重啟你的 Deno 程式。

  如果希望關閉該功能，我們可以在配置文件中加入 `"watch": false` :

  ```text
  {
    "watch": false
    "scripts": {
      "start": "deno run -A --unstable app.ts"
    }
  }
  ```

* 導入 MAP

  > 詳細訊息請參考[官方文件](https://deno.land/manual/linking_to_external_code/import_maps)或是 [Day 19](https://ithelp.ithome.com.tw/articles/10248827)。

  ```text
  {
    "scripts": {
      "start": {
        "cmd": "deno run app.ts",

        "importmap": "importmap.json"
      }
    }
  }
  ```

* 載入 tsconfig.json

  載入 tsconfig.json 以控制 TypeScript Compiler :

  ```text
  {
    "scripts": {
      "start": {
        "cmd": "deno run app.ts",

        "tsconfig": "tsconfig.json"
      }
    }
  }
  ```

* 開啟 unstable

  ```text
  {
    "scripts": {
      "start": {
        "cmd": "deno run app.ts",

        "unstable": true
      }
    }
  }
  ```

> Denon 還有支援非常多可選選項，如果讀者有興趣可以參考今天的延伸閱讀。

### 總結

會在這邊先介紹 Denon ，是因為我自己在開發 Web API 時會頻繁的修改並重新啟動程式碼，一直反覆地敲啟動命令跟 `clear` 也是會讓人厭煩的 XDD

老實說，在介紹 Denon 之前我自己也不知道它提供了這麼多額外選項，筆者本來的配置文件長這樣:

```text
{
    "$schema": "https://deno.land/x/denon/schema.json",
    "env": {
    },
    "scripts": {
      "start": {
        "cmd": "deno run --allow-env --allow-net --allow-write --allow-read --allow-plugin --unstable app.ts"
      }
    }
  }
```

我想，這就是分享技術文章的好處。在分享的同時也可以不斷整理思緒以及既有的知識，準備這次的鐵人賽也讓筆者自己重新學了一遍 JavaScript ，真的非常超值 Q\_Q

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [Denon](https://deno.land/x/denon@2.4.4)
*  [Deno: Create a Rest API using JWT](https://levelup.gitconnected.com/deno-create-a-rest-api-using-jwt-5141fd5b1066)

此外，今天是本系列的第 30 篇，雖然筆者還沒完賽，不過大家可以去看看我隊友們的文章唷!

*  [行銷廣告、電商小編的武器，FB & IG 爬蟲專案從零開始](https://ithelp.ithome.com.tw/users/20103256/ironman/2940)
*  [LeetCode30](https://ithelp.ithome.com.tw/users/20129147/ironman/2965)
*  [索引結構與機器學習的相遇](https://ithelp.ithome.com.tw/users/20129198/ironman/3001)

