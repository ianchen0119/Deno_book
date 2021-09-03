# 使用多線程計算矩陣相乘

在介紹完多線程的概念以及如何在 Deno 上實現多線程後，就讓我們來實做一些（不）好玩的城式吧！

### 題目

> 題目來源： 我大北科計算機牛逼組的作業系統課程作業。

1. Coding _independent threads_ can speed up the running of programs. Let us conduct an ex- periment to experience the features and skill of _multithreading_. You need to _code two differ- ent styles of programs_ to perform a “matrix multiplication” in the same programming lan- guage \(e.g. C language\), as follows.

![](https://i.imgur.com/EUOCJtE.png)

![](https://i.imgur.com/tSdayo5.png)

![](https://i.imgur.com/k8bq9NE.png)

首先，依照題目要求，我們需要先生出兩個矩陣，筆者在這邊寫了一個 CallBack Function ，讓它輸入位置後可以直接回傳矩陣中該位置的值：

```text
// index.ts
let a = function(i: number,j: number): number{
	return 3.5*i+(-6.6*j)
	}
function b(i: number,j: number) :number{
	return 6.6+(8.8*i)-3.5*j
	}
```

接著，我們寫一個 Function 讓它可以實現將兩個不同矩陣中的值相乘並回傳其值：

```text
// index.ts
let a = function(i: number,j: number): number{
	return 3.5*i+(-6.6*j)
	}
function b(i: number,j: number) :number{
	return 6.6+(8.8*i)-3.5*j
	}
function MatrixC(i :number,j :number){
	let result = 0;
	for (var x = 1; x <= 60; x++) {
		let c = a(i,x)*b(x,j)
		result = result + c;
	}
	return result
}
export { a,b,MatrixC }
```

題目中要求我們需要分別使用 `for-loop` 、 `multi-thread` 的方法實現出功能。

#### For-loop

```text
import { MatrixC } from './index.ts';
let count = 0;
function run(){
	for (var i = 1;i<=35; i++) {
		for (var j = 1; j<=35; j++) {
		let result = MatrixC(i,j);
		console.log(`C[${i},${j}]:${result}`)
        }
	}
}
```

我們可以直接使用雙重迴圈暴力硬解。

#### Multi-Thread

像是昨天教學中 `main.ts` 負責新增線程、 `worker.ts` 負責單一任務，我們在本題目的實作也依照這個邏輯拆分出兩個檔案：

```text
// main.ts
(function(){
	for (var i = 1;i<=35; i++) {
	const worker = new Worker(new URL("worker.ts", import.meta.url).href, { type: "module" });
	worker.postMessage(i); 
	}
})();
```

```text
// worker.ts
import { MatrixC } from './index.ts';

self.onmessage = (e) => {
	for (var j = 1;j<=35; j++) {
			let result = MatrixC(e.data,j);
			console.log(`C[${e.data},${j}]:${result}`)
		}
	self.close();
  };
```

實作上的邏輯非常簡單，筆者只是將 `for-loop` 版本的第一層迴圈拆掉並改用 `workers` 實作，當初在做這一題的時候花費最多時間的其實是複習矩陣相乘，而不是實現邏輯 XD

#### 多線程真的比較快嗎？

如果各位讀者也跟著實作了該範例，會發現 `for-loop` 的版本其實快上不少...

最根本的原因是在這次多線程的實作上開了太多執行緒，反而拖累了程式的執行效率，如果讀者有興趣也可以改成派發 `5` 、`7` 個線程並在 `worker.ts` 上多加一層 `for-loop` ，如果發現效果有顯著提升歡迎到留言區分享（？）

> 或是各位讀者也可以改用 C 語言實作，速度真的快上非常多 ORZ

### 總結

會特別花多達三篇的篇幅介紹多線程，其實是因為筆者想要跟大家分享著名的 [C10K 問題](https://en.wikipedia.org/wiki/C10k_problem)，該問題在20年前被提出，主要的問題大概是：

> 即使硬體的性能符合要求，伺服器仍無法同時處理 10000 個客戶端的連線請求，也就是 10000 個並發連結問題。

其實多線程就是解決並發連線問題的方法之一，原因筆者也在前天的文章提到：

> 響應快，不像單一執行緒會因為單一執行緒停擺造成應用程式崩潰。

商業邏輯只要是學過程式語言的人都能實現，但是並不是人人都能將資料結構、演算法或是計算機科學領域的知識應用到其中。這也是筆者希望藉由這次鐵人賽向其他跨領域的程式新手傳達的知識。

> 當然啦！板上有很多比筆者我還強的大大，如果文章內容有誤，也非常歡迎來信/留言指教。

最後，其實 C10K 問題已經退流行了，現在的頂尖開發者要面對的應該是 C10M 問題（1000萬人同時連線）XDD

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [程序员怎么会不知道 C10K 问题呢？](https://ithelp.ithome.com.tw/articles/%5Bhttps://medium.com/@chijianqiang/%E7%A8%8B%E5%BA%8F%E5%91%98%E6%80%8E%E4%B9%88%E4%BC%9A%E4%B8%8D%E7%9F%A5%E9%81%93-c10k-%E9%97%AE%E9%A2%98%E5%91%A2-d024cb7880f3%5D%28https://medium.com/@chijianqiang/%E7%A8%8B%E5%BA%8F%E5%91%98%E6%80%8E%E4%B9%88%E4%BC%9A%E4%B8%8D%E7%9F%A5%E9%81%93-c10k-%E9%97%AE%E9%A2%98%E5%91%A2-d024cb7880f3%29)

