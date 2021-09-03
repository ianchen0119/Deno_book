# 程式碼格式化

### 程式碼格式化

該功能是基於 [dprint](https://github.com/dprint/dprint) 實作出來的，至於什麼是格式化呢?

我們先來看亂七八糟的程式碼:

```text
function sum(){
let a = 10;
	let b =20;
return a+ b;
}
```

上面是所謂的亂器八糟程式碼，假設筆者今天就是這種程式碼的製造者，想要洗心革面卻又不知道該從何下手，

在 Deno 中，就提供了這樣的工具可以幫你一鍵整理程式碼 \(支援 TypeScript 以及 JavaScript \):

```text
# format all JS/TS files in the current directory and subdirectories
deno fmt
# format specific files
deno fmt myfile1.ts myfile2.ts
# check if all the JS/TS files in the current directory and subdirectories are formatted
deno fmt --check
# format stdin and write to stdout
cat file.ts | deno fmt -
```

#### 忽略格式化

如果使用者不希望有些程式碼被打理的漂漂亮亮\(?\)，可以這麼做:

1. 在程式碼前面加上 `// deno-fmt-ignore` :

   ```text
   // deno-fmt-ignore
   export const identity = [
       1, 0, 0,
       0, 1, 0,
       0, 0, 1,
   ];
   ```

2. 直接在程式碼頂部加上 `// deno-fmt-ignore-file`

