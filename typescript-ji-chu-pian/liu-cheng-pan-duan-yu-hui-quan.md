---
description: 在瞭解如何宣告變數之後，就來跟者讀者一起學習撰寫程式時會經常使用到的流程判斷以及迴圈功能吧!
---

# 流程判斷與迴圈

### 流程判斷

要做流程判斷，就必須瞭解`條件語法`這項酷東西。

> 條件語法的意思是 : 當條件成立時，就執行這些指令。

至於編譯器是如何判斷條件是否成立的呢?先前在變數宣告篇介紹的布林值 \(boolean\) 就派上用場啦!

> 情境題 : 假設小明今天想要在 Apple 直營門市購買全新的 Iphone 並且他將 Iphone 11 Pro 作為購買目標。
>
> 至於手機容量的大小呢 ? 就取決於荷包的厚度了。

```text
let TwdIsPower: number = 36000; //新台幣就是力量
if(TwdIsPower>48400){
   console.log("就決定是你了! 512GB的Iphone!")
   }
else if(TwdIsPower>41400){
   console.log("就決定是你了! 256GB的Iphone!")
        }
else if(TwdIsPower>35900){
   console.log("就決定是你了! 64GB的Iphone!")     
        }
else{
   console.log("下個月再來吧! 窮鬼!") 
}
```

根據上面的範例，小明的荷包只有 36000 元，因此他會符合第三項條件，選擇購買 64GB 的 Iphone 。

> 如果今天的狀況更簡單，也就是非黑即白的狀況。
>
> 情境題 : 小明有 36000 元，不過他忘記帶錢了。

```text
let mingHaveMoney: boolean = false; //小明有帶錢ㄇ? 沒有。
if(mingHaveMoney){
    console.log("就決定是你了! 64GB的Iphone!")   
}
else{
   console.log("回家拿錢再來吧! 冒失鬼!") 
}
```

當今天的條件不是 A 就是 B 時，我們也可以考慮使用更為精簡的寫法:

```text
let mingHaveMoney: boolean = false; //小明有帶錢ㄇ? 沒有。
mingHaveMoney?(console.log("就決定是你了! 64GB的Iphone!")):(console.log("回家拿錢再來吧! 冒失鬼!"));
```

`?` 前的表達式為條件， `?` 後的表達式為條件成立時執行的程式， `:` 後的則為條件不成立時執行的程式。

#### 使用 switch

當今天有多個條件 \(2個以上\)，我們也可以考慮使用 `switch` 去撰寫條件式。

```text
let TwdIsPower: number = 36000; //新台幣就是力量

switch ( TwdIsPower ){
  case 48400:
    console.log("就決定是你了! 512GB的Iphone!")
    break;
  case 41400:
    console.log("就決定是你了! 256GB的Iphone!")
    break;
  case 35900:
    console.log("就決定是你了! 64GB的Iphone!")
    break;
  default:
    console.log("下個月再來吧! 窮鬼!") 
}
```

* 當 `case` 後面的值等於輸入的值，便會執行 `case` 所包含的程式。
*  `case` 內的程式在結束時都需要以 `break` 作為結尾，因為在 `switch` 中，並沒有使用大括號 `{}` 將不同條件區隔開來。
* 當所有 `case` 都沒有成立，便會執行 `default` 。

### 迴圈

> **迴圈**是[計算機科學](https://zh.wikipedia.org/wiki/%E8%A8%88%E7%AE%97%E6%A9%9F%E7%A7%91%E5%AD%B8)運算領域的用語，也是一種常見的[控制流程](https://zh.wikipedia.org/wiki/%E6%8E%A7%E5%88%B6%E6%B5%81%E7%A8%8B)。迴圈是一段在程式中只出現一次，但可能會連續執行多次的程式碼。迴圈中的程式碼會執行特定的次數，或者是執行到特定條件成立時結束迴圈，或者是針對某一集合中的所有項目都執行一次。
>
> 在一些[函式程式語言](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B8%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80)（例如 [Haskell](https://zh.wikipedia.org/wiki/Haskell) 和 [Scheme](https://zh.wikipedia.org/wiki/Scheme)）中會使用[遞迴](https://zh.wikipedia.org/wiki/%E9%80%92%E5%BD%92_%28%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6%29)或[不動點組合子](https://zh.wikipedia.org/wiki/%E4%B8%8D%E5%8A%A8%E7%82%B9%E7%BB%84%E5%90%88%E5%AD%90)來達到迴圈的效果，其中[尾端遞迴](https://zh.wikipedia.org/wiki/%E5%B0%BE%E9%83%A8%E9%80%92%E5%BD%92)是一種特別的遞迴，很容易轉換為[疊代](https://zh.wikipedia.org/wiki/%E8%BF%AD%E4%BB%A3)。
>
> -- [wikipedia](https://zh.wikipedia.org/wiki/%E8%BF%B4%E5%9C%88)

簡單來說: 如果我們今天希望能讓程式碼重複的執行某件事，其數值可能會依據重複的次數產生變化 \(遞增 or 遞減\)並且滿足迴圈跳脫的條件。

#### for 迴圈

```text
for(初始值; 條件; 結束時變動){
    //do something...
}
```

> 情境題 : 半夜睡不著 ? 你應該考慮數羊而不是聽[屋頂](https://www.youtube.com/watch?v=lHT_F3h6_BY)。

```text
console.log(一隻);
console.log(兩隻);
console.log(三隻);
// ...
```

诶不是!這樣做好像有一點呆耶，那如果我們用迴圈呢?!

```text
for(var i=1; i=<10; i++){
console.log(`${i}隻`);
}
```

* 知識點: [樣板字串](https://wcc723.github.io/javascript/2017/12/22/javascript-template-string/)

> 解釋 : `i` 的初始值為 1 ，當滿足 `i<11` 的條件便會執行 `{}` 的程式碼並且在該圈結束時對 `i` 的值加上 1 。

這樣一來，我們就能在終端機看到:

```text
PS C:\Users\IAN\Desktop\ItIronMan2020-Hello-Deno\Example> deno run .\for.ts
Check file:///C:/Users/IAN/Desktop/ItIronMan2020-Hello-Deno/Example/for.ts
1隻
2隻
3隻
4隻
5隻
6隻
7隻
8隻
9隻
10隻
```

#### while 迴圈

```text
var i = 0; //初始值
while(條件;){
    //do something...
    i++; //結束時變動
}
```

筆者將上面的範例使用 `while` 改寫看看:

```text
var i = 1;
while(i<=10){
	console.log(`${i}隻`);
    i++;
      }
```

```text
PS C:\Users\IAN\Desktop\ItIronMan2020-Hello-Deno\Example> deno run .\for.ts
Check file:///C:/Users/IAN/Desktop/ItIronMan2020-Hello-Deno/Example/for.ts
1隻
2隻
3隻
4隻
5隻
6隻
7隻
8隻
9隻
10隻
```

恩，就結果來看，同樣可行!

#### 無窮迴圈

> 注意 ! 在使用迴圈時必須確保`執行條件`的正確性，否則會造成無窮迴圈的產生，像是:
>
> ```text
> while(true){
>  console.log("爆炸啦!!!");
> }
> ```
>
> 在上面的範例中，因為條件永遠成立 \(True\) ，因此，當程式執行時將會從命令列中不斷地噴出`爆炸啦!!!`。
>
> 如果讀者想要玩看看，歡迎服用 XD

此外，之前在學校開設的`作業系統`課程中，老師要我們測試看看自己的電腦是否是個狠腳色。

因此，要求我們使用 `chrome` 開啟指定網頁並紀錄電腦能夠同時開啟多少個視窗直到 `chrome` 崩潰。

如果你真的傻傻的把網頁一個一個打開來，那這份作業你恐怕會做到天亮 XDD

> Hint: 這個時候，無窮迴圈就派上用場啦 : \)

[FuckOs](https://github.com/ianchen0119/FuckOs)

> BTW: 該 Repo 中使用 `setInterval()` 下去實現，原因是迴圈的噴發速度太快，我的肉眼跟不上 `console` 噴發訊息的速度 XDDD
>
> 如果覺得這個作業很ㄎㄧㄤ，歡迎讀者發 `PR` 或是按下 `star` 灌爆我吧 !!!

#### break 以及 continue

* break 會讓編譯器直接跳過該迴圈
* continue 會讓編譯器跳過一次迴圈，然後繼續執行迴圈。

差別在哪呢? 如果筆者希望數羊數到 100 次，可以這樣做:

```text
var i = 1;
while(i<=100;i++){
	console.log(`${i}隻`);
      }
```

或是利用 `break` ，讓它在 `i = 100` 時跳出迴圈:

```text
var i = 1;
while (true) {
  console.log(`${i}隻`);
  if (i === 100) {
    break;
  }
  i++;
}
```

如果筆者突然很討厭某一隻羊，不希望牠被數出來呢?

> 等等，這舉例也太爛了吧 xDDD

那可以使用 `continue` 達到筆者的需求:

```text
var i = 1;
while (i <= 10) {
  if (i === 3) {
    i++;
    continue;
  }
  console.log(`${i}隻`);
  i++;
}
```

> 注意 ! 使用 `continue` 時需要格外注意 ! 在上面的範例中 :
>
> ```text
> if (i === 3) {
>     i++;
>     continue;
>   }
> ```
>
> 做了 `i++` 的動作，其目的是為了讓此次迴圈在跳出之前將 `i` 變數的值增加，以避免下一輪迴圈還是在該區塊執行進而導致無窮迴圈的產生。

#### 無窮迴圈產生了怎麼辦?

如果讀者因為程式構思不全而引發無窮迴圈的發生，只需要在執行程式的命令列按下 `ctrl` + `c` 便能解決，不需要太擔心。

> 當然，你也可以考慮讓電腦重新開機冷靜一下。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* 重新認識 JavaScript

  > 重新認識 JavaScript 是由台灣 Vue.js 社群的大大 - Kuro 所編寫，該系列將許多重要觀念羅列出來並且寫的淺顯易懂，十分牛逼。

  * [流程判斷與迴圈](https://ithelp.ithome.com.tw/articles/10191453)
  * [比較與自動轉型](https://ithelp.ithome.com.tw/articles/10191254)

