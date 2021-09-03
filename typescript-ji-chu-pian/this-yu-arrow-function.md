# This 與 Arrow Function

在前一篇章節，筆者介紹了函式的概念以及多種函式的宣告方法、立即函式，本章我們就來進一步探討什麼是箭頭函式、函式的呼叫方法有哪些以及 `this` 的概念吧！

### What is "This"?

若讀者不是第一次寫程式，並且有接觸過物件導向程式語言，對於 `this` 肯定不陌生，像是下方的 PHP 程式碼：

```text
//...
class linkedList implements linkedListInterface{
    private $head = 0;
    private $length = 0;
    private $memory = array();
    function printList(){
        if($this->length ==0){
            return 0;
        }
        else{
            $currentNode = $this->head;
         while(true){
            print_r($this->memory[$currentNode]);
            $currentNode = $this->memory[$currentNode]->getNext();
            if($currentNode === "nullPtr"){
            break;
                }
            }
            print "
";
        }
    }
    //...
    }
//...
```

在大多數的程式語言中， `this` 都指向其**實體本身**。不過，身為怪奇語言的 JavaScript 肯定是不會放過廣大開發者的ＸＤ

在 JavaScript 中，`this` 會隨著函式、 `class` 實體化時被自動生成，它通常代表呼叫函式的那個物件，像是：

```text
const returnYourName = function(){
	return this.name;
}
const Ian = {
  name: 'Ian',
  whatYourName: returnYourName
}
const Peter = {
  name: 'Peter',
  whatYourName: returnYourName
}
console.log(Ian.whatYourName()) //Ian
console.log(Peter.whatYourName()) //Peter
```

我們可以透過結果發現：

當執行 `Ian.whatYourName()` 時， 在 `Ian` 物件中的 `whatYourName()` 的函式實體的 `this` 是指向 `Ian` 的。

這也驗證了這句話：

> 在 JavaScript 中，`this` 會隨著函式、 `class` 實體化時被自動生成，它通常代表呼叫函式的那個物件。

如果讀者不相信，我們可以進一步透過程式碼驗證：

```text
const returnYourName = function(){
	return this.name;
}
const returnThis = function(){
	return this.name;
}
const Ian = {
  name: 'Ian',
  whatYourName: returnYourName,
  returnThis: returnThis
}
console.log(Ian.returnThis()) //Ian
```

結果也與其他說法如出一轍。

### call\(\), apply\(\), bind\(\)

在上述的範例中，我們已經知道：

> 在 JavaScript 中，`this` 會隨著函式、 `class` 實體化時被自動生成，它通常代表呼叫函式的那個物件。

不過， JavaScript 也有提供三種方法讓我們自己決定 `this` 該指向誰。

在正式開始之前，我們先宣告一個 `sum` 函式：

```text
let sum = function(para1, para2){    
  return para1 + para2; 
} 
```

在一般情況下，若我們要呼叫定義好的函式會這麼做：

```text
sum(1,2); //3
```

#### bind\(\)

我們可以利用 `bind()` 讓函式的 `this` 綁定我們指定的值，如下：

```text
function sum(para1:number,para2:number) {
  console.log(para1 + para2,this)
}

const sumX = sum.bind('Hello')
sumX(1,2)
```

根據範例程式碼，筆者宣告了 `sumX` 存放綁定 `this` 後的 `sum` 函式，所以我們在呼叫 `sumX()` 時，便會透過命令列看到：

```text
3, Hello
```

#### apply\(\) and call\(\)

與 `bind()` 不同， `apply()` 和 `call()` 只需要在呼叫函式時加在後方：

```text
sum.call('Ian', 1, 2) // Ian 1 2
sum.apply('Peter', [1, 2]) // Peter 1 2
```

至於 `call()` 與 `apply` 的差別，就只是帶入參數的方法不同而已。

### 箭頭函式

在先前的篇章中，筆者僅介紹了普通函式。在 JavaScript ES6 開始新增了一個特別的函式： 箭頭函式 \(Arrow function\)。

使用起來非常簡潔：

```text
()=>{
console.log("Hello, world!")
}
let sum = (para1: number, para2: number)=>{
  return para1 + para2;
}
let result = sum()
```

需要注意的是，箭頭函式的 `this` 與一般函式不同，箭頭函式的 `this` 會綁定到其定義時所在的物件。

> 因為筆者在準備範例時在 Deno 上遇到了一些問題，所以會先將這部分放到問題解決再補上。
>
> 如果想進一步學習也可以參考今天的延伸閱讀。

### 目前遇到的問題

上面提到筆者遇到了一些問題，為了證明筆者不是在找藉口不寫稿，筆者就把目前遇到的問題丟上來：

* 全域下， `this` 應該要是 `window` 而不是 `undefined` 。
* 一直噴以下錯誤：

```text
TS2683 [ERROR]: 'this' implicitly has type 'any' because it does not have a type annotation.
 return this.name;
```

在 Deno 的官方文件中有提到，在全域中的 `this` 為 `window` \(node.js 為 `global`\)，然而不論筆者怎麼嘗試，結果都是 `undefined` ，我也有考慮到是否為 TypeScript 預設會開`嚴格模式`，不過筆者也嘗試了[官方文件](https://deno.land/manual/getting_started/typescript)中提到的 `-c tsconfig.json` 與 `--no-check`。  
 當加上 `--no-check` 執行程式碼時，確實不會在噴出 `'this' implicitly has type 'any' because it does not have a type annotation.` 的錯誤，不過筆者認為這項作法只是在逃避問題，並不是好作法。  
 至於在全域中的 `this` 為 `undefined` 的問題，我在 `tsconfig.json` 將嚴格模式中把它關掉以後也不見改善，我在想一定是我沒有將官方文件閱讀清楚，還請各位讀者諒解，我會盡快找出問題的答案，如有必要，也會在 `Deno` 的 Repo 發看看 `issue` （但我認為這問題不是 Deno 本身造成的，所以筆者還是希望能自己解決問題）。

### 延伸閱讀

> 同樣的事情在不同人眼中可能會有不同的見解、看法。
>
> 在讀完本篇以後，筆者也強烈建議大家去看看以下文章，或許會對型別、變數宣告...等觀念有更深層的看法唷！

*  [鐵人賽：箭頭函式 \(Arrow functions\)](https://wcc723.github.io/javascript/2017/12/21/javascript-es6-arrow-function/)
*  [淺談 JavaScript 頭號難題 this：絕對不完整，但保證好懂](https://blog.techbridge.cc/2019/02/23/javascript-this/)

