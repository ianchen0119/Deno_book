---
description: 本篇為 Deno 的安裝教學。
---

# Hello, World!

### 搭建開發環境

> 筆者本人是使用 ＭacOS ，因此範例的圖片也是從 MacOS 所擷取下來的。
>
> 其他作業系統會在一部分的步驟上有些許的不同，本文一樣會把它點出來。

尷尬的事來了，其實安裝 Deno 十分容易，基本上一行指令搞定：

> 讀者現在一定會覺得：那你剛剛跟我閒扯這麼多幹嘛呢？
>
> 其實是因為我是一邊寫文章一邊初體驗 Deno 的，我認爲這樣子所產出的文章會有更生動的感覺。

1. 打開你的 CMD 或是終端機。
2. 根據不同的作業系統，會有不同的命令需要輸入：

   Shell \(Mac, Linux\):

   ```text
   $curl -fsSL https://deno.land/x/install/install.sh | sh
   ```

   PowerShell \(Windows\):

   ```text
   $iwr https://deno.land/x/install/install.ps1 -useb | iex
   ```

   [Homebrew](https://formulae.brew.sh/formula/deno) \(Mac\):

   ```text
   $brew install deno
   ```

   [Chocolatey](https://chocolatey.org/packages/deno) \(Windows\):

   ```text
   $choco install deno
   ```

   Scoop \(Windows\):

   ```text
   $scoop install deno
   ```

   Build and install from source using [Cargo](https://crates.io/crates/deno)

   ```text
   $cargo install deno
   ```

3. 大功告成，一切就是這麼的簡單、愉快。

   ![2-1](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch2/2-1.png)

> 上述指令都是由Deno Doc所提供，[來源](https://deno.land/#installation)在這裡。

### Hello, World! 

#### 快速開始

![](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch2/2-2.png)

> 如果筆者遇到如下圖一樣的問題可以參考：
>
> ![2-3](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch2/2-3.png)
>
> 系統竟然找不到 deno 這道指令，怎麼辦呢？
>
> 放心，這只是因為我們沒有設定環境變數所造成的，我們只要參考[這篇文章](https://tute.io/install-deno-macos)設定之後，便能順利執行囉～！
>
> 貼心提醒：做完`Manually add the directory to your $HOME/.zshrc or $HOME/.bash_profile`這個步驟時，請記得下`source ~/.bash_profile`這道指令。

#### 欲善其事 必先利其器

要做出好的產品，我們勢必需要好的工具，所以，在開始之前，請先到 VSCode 官網下載並安裝好這套強大的編輯器。

連結在這裡：[VSCode](https://code.visualstudio.com/)

#### 進入正題

終於....我們可以開始編寫 Hello, World! 程式了...

1. 首先，我們創建一個新檔案，其副檔名為 `ts` 。

   ![2-5](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch2/2-5.png)

2. 使用 VSCode 打開你剛剛創建的檔案，並輸入：

   ```text
   console.log("Hello, World!")
   ```

   輸入好後，請記得存檔。

3. 打開我們的終端機輸入：

   ```text
   deno run YourCode.ts
   ```

   > 貼心提醒：VSCode 內建終端機，如果要呼叫它請按組合鍵：
   >
   > `Ctrl`+_\`_

4. 查看結果

   ![2-6](https://github.com/ianchen0119/ItIronMan2020-Hello-Deno/raw/master/ch2/2-6.png)

5. 做到這邊，筆者實在是感動到說不出話了，看似簡單的幾個步驟，竟然花了我一個多小時....Q\_Q

### Hello, World! 的由來

花了這麼多時間進行 `Hello, World!` 儀式，那就順便介紹一下它的歷史吧！

> 於1972年，[貝爾實驗室](https://zh.wikipedia.org/wiki/%E8%B2%9D%E7%88%BE%E5%AF%A6%E9%A9%97%E5%AE%A4)成員[布萊恩·柯林漢](https://zh.wikipedia.org/wiki/%E5%B8%83%E8%90%8A%E6%81%A9%C2%B7%E6%9F%AF%E6%9E%97%E6%BC%A2)撰寫的內部技術檔案《A Tutorial Introduction to the Language B》首次提到了 Hello World 。當時，他使用[B語言](https://zh.wikipedia.org/wiki/B%E8%AA%9E%E8%A8%80)撰寫了第一個使用參數的 Hello World 相關程式：
>
> ```text
> main(){
>   extrn a,b,c;
>   putchar(a); putchar(b); putchar(c); putchar('!*n');
>   }
>
> a 'hell';
> b 'o, w';
> c 'orld';
> ```
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/Hello_World)

兩年後，世人所熟知的 C 語言也誕生了，作者[布萊恩·柯林漢](https://zh.wikipedia.org/wiki/%E5%B8%83%E8%90%8A%E6%81%A9%C2%B7%E6%9F%AF%E6%9E%97%E6%BC%A2)也讓 C 語言能以更簡單的方式讓大家展示出 `Hello, World!`:

```text
#include 
main( )
{
	printf("hello, world\n");
}
```

> 需要注意的是， Hello World 的初始寫法為「hello, world」，並沒有任何感嘆號，全部都是小寫，內含逗號，後面亦有空格，而和現在流行的寫法並不一致。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/Hello_World)

從此之後， `Hello, World!` 成為每本程式語言必定出現的章節之一，本系列文更是無聊到將這個部分獨立成一篇章節來介紹XDD  


