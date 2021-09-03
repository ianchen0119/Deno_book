# 多線程概念

### 進入正題

在 Operating system 的學科中有提到：

* Program
* Process
* Thread

至於他們之間的關係呢？筆者可以舉一個實際的例子：

不論是我們平常撰寫的程式碼，或是大眾常使用的 APP, 瀏覽器...等等，都可以稱為 Program 。

當我們開啟應用程式時，作業系統會將它載入到 RAM 當中，此時位於 RAM 待執行/執行中的程式稱為 Process 。

![Process &#x7684;&#x4E94;&#x7A2E;&#x72C0;&#x614B;](https://3.bp.blogspot.com/-HNdi1toXPr4/VLR8Mm1XHYI/AAAAAAAAlAc/lor1HoGCCBE/s1600/%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%2B2015-01-13%2B%E4%B8%8A%E5%8D%889.45.25-51.png)

而每個獨立的 Process 都有自己的管理員，叫做： PCB \(process control box\) 。

PCB 負責管理 Process 的狀態、程式計數器、CPU 暫存器、 CPU 排程資訊...等。

實際上，作業系統會管理 PCB，而 PCB 會負責管理 Process 。現在假設 Process A 需要 Process B 幫忙處理其他事情：

1. PA 被中斷或是等待 System Call
2. PA 的相關狀態會全部存取到 PCB A
3. PCB A 會將資源與 PCB B
4. PB 載入 PCB B 的資源
5. 執行 PB

#### 程序堵塞的狀況

以瀏覽器為例，我們都知道 JS 引擎其實只有單一執行緒。因此，當開發者因設計錯誤產生無窮迴圈，抑或是產生太多的任務將 JS 的執行序列卡住，都會造成網頁沒有任何回應，像是：按下傳送按鈕沒有回應、SPA 無法換頁...等等。

> 注意：是網頁沒有任何回應，不是瀏覽器沒有回應唷！

雖然 JS 引擎是單一執行緒，但是它還是透過了聰明的設計、配合瀏覽器 API 做到非同步處理。

以 Chrome 瀏覽器為例，我們在瀏覽器上面執行下列程式碼：

```text
(function(){
  console.log('start');
	setTimeout(()=>{console.log('done!')};,5000);
	console.log('Waiting for 5s...');
})();	
```

程式碼執行時，上面的程式碼會被一一讀取，並且將有需要處理的任務放到 Stack 當中：

1.  `console.log('start');`被放到 Stack 中並且被即時處理。
2.  `setTimeout(()=>{console.log('done!')})` 被放到 Stack 中，因為該 API 由瀏覽器實作，所以將計時任務丟到 web api 處理。
3.  `console.log('Waiting for 5s...');` 被放到 Stack 中並且被即時處理。
4. 等待 web api 計時 5 秒，將 `console.log('done!')` 丟到 queue 中。
5. stack 已經沒有任務待處理， event loop 將 queue 的任務放到 stack 中。
6.  `console.log('done!')` 完成處理。

* 知識點： [Stack 與 Queue](https://www.csie.ntu.edu.tw/~r95116/CA200/slide/C8_StackQueue.pdf)

  它們都是一種資料結構，與 Array 很像，可以存放多筆資料，不同的是， Queue 為 FIFO ， Stack 為 FILO 。

#### 什麼是多執行緒 \(Multi-threads\)

剛剛介紹了 Process 以及 Program ，至於 Thread 呢？

Thread 就是負責將 Process 完成的那位要角。一個 Process 是可以分配多個 Thread 去執行任務的，考慮實際應用面，我們以 Server 程式為例：

當前端程式像後端程式發起請求，若使用多線程開發，這時 Process 就可以去分派多個 Thread 去處理不同的使用者請求，以防止單執行緒時會有程序堵塞的狀況。  
 回到正題， 多執行緒的應用有很多好處，像是：

* 響應快，不像單一執行緒會因為單一執行緒停擺造成應用程式崩潰。
* 資源的分享、使用較 Process 容易、少。
* 擴展性好：在多處理器架構下，可以做到平行處理。

聰明的讀者可能會問：

單核心處理器能做到多線程嗎？

~~可以，當然可以，哪次不可以。~~

我們可以利用分時多工的概念讓單核心支持多線程，不過因為本系列文並不是作業系統原理概論，所以就先不一ㄧ介紹啦。

### 總結

筆者會在 Deno Workers 之前特別獨立一篇文章說明 CS 領域的學科原理，不外乎是希望讀者不要只學會如何使用工具，身為技術人應該要努力理解技術背後的原理。

當我修了資料結構、演算法以及作業系統後，開始發現我們日常使用的應用程式或是開發者用到的 Library, Tool, Framework 都大量使用了相關知識。

確實，不懂 Queue 跟 Stack 也可以寫出商業邏輯。但我認為學習相關知識後不管是在撰寫程式碼、閱讀原始碼時都會有更多不同的感受。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

* [所以說event loop到底是什麼玩意兒？\| Philip Roberts \| JSConf EU](https://www.youtube.com/watch?v=8aGhZQkoFbQ&index=42&list=WL&t=32s)
* [PJ 大的筆記](https://pjchender.blogspot.com/2017/08/javascript-learn-event-loop-stack-queue.html)
* [OS作業系統學習 系列文](https://ithelp.ithome.com.tw/users/20112132/ironman/1884)

  > 該系列文是 OS 原文書的相關心得，至於筆者為何知道呢？因為當初在修 OS 準備期中期末考時，我都會混搭該系列文惡補 XDD 因此在這邊推薦給各位讀者！

