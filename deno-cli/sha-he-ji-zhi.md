# 沙盒機制

> 在[電腦安全](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%AE%89%E5%85%A8)領域，**沙盒**（英語：sandbox，又譯為**沙箱**）是一種安全機制，為執行中的程式提供的隔離環境。通常是作為一些來源不可信、具破壞力或無法判定程式意圖的程式提供實驗之用\[[1\]](https://zh.wikipedia.org/wiki/%E6%B2%99%E7%9B%92_%28%E9%9B%BB%E8%85%A6%E5%AE%89%E5%85%A8%29#cite_note-1)。
>
> 沙盒通常嚴格控制其中的程式所能存取的資源，比如，沙盒可以提供[用後即回收](https://zh.wikipedia.org/wiki/%E5%A1%97%E9%8A%B7%E7%A9%BA%E9%96%93)的磁碟及記憶體空間。在沙盒中，網路存取、對真實系統的存取、對輸入裝置的讀取通常被禁止或是嚴格限制。從這個角度來說，沙盒屬於[虛擬化](https://zh.wikipedia.org/wiki/%E8%99%9A%E6%8B%9F%E5%8C%96)的一種。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/%E6%B2%99%E7%9B%92_%28%E9%9B%BB%E8%85%A6%E5%AE%89%E5%85%A8%29)

Sandbox 讓軟體執行於一個受限的系統環境中，控制程式可使用的資源，而這同樣也是 Deno 的特色之一。

本篇筆者要向大家分享的就是導入 Sandbox 概念的 Deno 權限旗標。

### 進入正題

Deno 的官方文件中提到：

> Deno is secure by default. Therefore, unless you specifically enable it, a deno module has no file, network, or environment access for example. Access to security-sensitive areas or functions requires the use of permissions to be granted to a deno process on the command line.

Deno 是預設安全的執行環境，因此，除非使用者允許開啟， Deno 的模組（包含開發者的程式碼）並不具有檔案、網路、環境存取權。

以簡單的 web server 程式碼為例：

```text
/* 
 * webserver.ts 
 */
import { serve } from "https://deno.land/std@0.71.0/http/server.ts";

const server = serve({ hostname: "0.0.0.0", port: 8080 });
console.log(`HTTP webserver running.  Access it at:  http://localhost:8080/`);

for await (const request of server) {
  let bodyContent = "Your user-agent is:\n\n";
  bodyContent += request.headers.get("user-agent") || "Unknown";

  request.respond({ status: 200, body: bodyContent });
}
```

在執行時，命令必須包含 `--allow-net` 才能順利執行：

```text
deno run --allow-net webserver.ts
```

> 在該系列文中，旗標跟子命令的意思是相同的，只是筆者不知道該用哪個形容會更好 XD

### 權限列表

除了剛剛看到的 `--allow-net` 以外， Deno 官方文件上還有提供詳細的列表：

* **-A, --allow-all** 允許所有權限。

  > 筆者不建議這麼做，因為使用它便失去 Sandbox 機制的意義了。

* **--allow-env** 允許環境訪問，像是：存取、設置環境變數。
* **--allow-hrtime** 允許高精度的時間測量，高精度時間測量可以用來定時攻擊、指紋識別（？）。
* **--allow-net=** 允許網路存取，此外使用者還可以選填允許存取的域名列表。
* **--allow-plugin** 允許加載插件。

  > 需要注意的是， `--allow-plugin` 是一個不穩定的功能，所以還需要加上 `--unstable` 標籤。
  >
  > 舉例：
  >
  > ```text
  > deno run --allow-plugin --unstable main.ts
  > ```

* **--allow-read=** 允許程式碼對檔案進行讀取，此外使用者還可以選填允許讀取的目錄、檔案列表。
* **--allow-run** 允許運行子進程。

  > 需要注意的是，由主進程開啟的子進程沒有在 sandbox 機制中運行，因此沒有與 deno 進程一樣具有相同的安全限制。必須謹慎使用。

* **--allow-write=** 允許程式碼對檔案進行寫入，此外使用者還可以選填允許寫入的目錄、檔案列表。

### 範例

在暸解 Deno 的 Sandbox 運作機制以後，有興趣的讀者可以將 Deno 官網中所提供的範例一一把玩：

*  [Manage dependencies](https://deno.land/manual/examples/manage_dependencies)
*  [Fetch data](https://deno.land/manual/examples/fetch_data)
*  [Read and write files](https://deno.land/manual/examples/read_write_files)
*  [Unix cat program](https://deno.land/manual/examples/unix_cat)
*  [HTTP web server](https://deno.land/manual/examples/http_server)
*  [File server](https://deno.land/manual/examples/file_server)
*  [TCP echo server](https://deno.land/manual/examples/tcp_echo)
*  [Creating a subprocess](https://deno.land/manual/examples/subprocess)
*  [OS Signals](https://deno.land/manual/examples/os_signals)
*  [File system events](https://deno.land/manual/examples/file_system_events)
*  [Module metadata](https://deno.land/manual/examples/module_metadata)

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [Deno Manual](https://deno.land/manual/getting_started/command_line_interface)

