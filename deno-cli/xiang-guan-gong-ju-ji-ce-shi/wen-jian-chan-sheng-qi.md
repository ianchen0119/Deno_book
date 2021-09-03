# 文件產生器

### 文件產生器

Deno 提供了一個十分酷炫的功能： 文件產生器！！

舉例來說，我們先創建一個 `add.ts` 文件：

```text
/**
 * Adds x and y.
 * @param {number} x
 * @param {number} y
 * @returns {number} Sum of x and y
 */
export function add(x: number, y: number): number {
  return x + y;
}
```

讓我們進一步觀察程式碼的註解區：

```text
/**
 * Adds x and y.
 * @param {number} x
 * @param {number} y
 * @returns {number} Sum of x and y
 */
```

我們在這邊看到優良的註解示範，使用 `@param {type} variable` 和 `@returns {type} Sum of x and y` 將變數和函式的定義交代的非常清楚。

這麼做的好處是，即使今天開發的是 JavaScript 應用， VS Code 也能讀懂你的程式碼。

> 如果讀者想要看更多關於如何撰寫好的 JS 註解的文件，可以參考[建立 JavaScript IntelliSense 的 JSDoc 註解](https://docs.microsoft.com/zh-tw/visualstudio/ide/create-jsdoc-comments-for-javascript-intellisense?view=vs-2015&viewFallbackFrom=vs-2019)。

接著，我們來看看 Deno doc 要如何使用：

```text
deno doc add.ts
```

Output:

```text
function add(x: number, y: number): number
  Adds x and y. @param {number} x @param {number} y @returns {number} Sum of x and y
```

#### 輸出成 JSON 格式

使用 `--json` 旗標可以讓 Deno doc 以 JSON 格式輸出：

```text
deno doc --json add.ts
```

Output:

```text
[
  {
    "kind": "function",
    "name": "add",
    "location": {
      "filename": "file:///Users/ianchen/Desktop/ItIronMan2020-Hello-Deno/Example/doc.ts",
      "line": 7,
      "col": 0
    },
    "jsDoc": "Adds x and y.\n@param {number} x\n@param {number} y\n@returns {number} Sum of x and y",
    "functionDef": {
      "params": [
        {
          "kind": "identifier",
          "name": "x",
          "optional": false,
          "tsType": {
            "repr": "number",
            "kind": "keyword",
            "keyword": "number"
          }
        },
        {
          "kind": "identifier",
          "name": "y",
          "optional": false,
          "tsType": {
            "repr": "number",
            "kind": "keyword",
            "keyword": "number"
          }
        }
      ],
      "returnType": {
        "repr": "number",
        "kind": "keyword",
        "keyword": "number"
      },
      "isAsync": false,
      "isGenerator": false,
      "typeParams": []
    }
  }
]% 
```

