# 程式碼檢查器

### 程式碼檢查器

如果讀者原本就有使用主流前端框架開發的經驗，對 ESLint 肯定不陌生（嗎？）。

Deno 內建了程式碼檢查器，可以用於檢查使用者的 TypeScript 和 JavaScript 程式碼是否符合規範。

```text
# lint all JS/TS files in the current directory and subdirectories
deno lint --unstable
# lint specific files
deno lint --unstable myfile1.ts myfile2.ts
# print result as JSON
deno lint --unstable --json
# read from stdin
cat file.ts | deno lint --unstable -
# get more details
deno lint --help
```

> 注意： 該功能還不是很穩定，所以在使用時一樣要加上 `--unstable` 唷！

#### 忽略指令

我們可以使用忽略指令來跳過不想被檢查的檔案：

```text
// deno-lint-ignore-file

function foo(): any {
  // ...
}
```

`// deno-lint-ignore-file` 可以選擇放在文件的最頂端或是第一次宣告的前方。

此外，我們也可以讓忽略指令用來跳過特定的規則的檢查：

```text
// deno-lint-ignore no-explicit-any
function foo(): any {
  // ...
}

// deno-lint-ignore no-explicit-any explicit-function-return-type
function bar(a: any) {
  // ...
}
```

最後， Deno 還特別兼容了 ESLint 的指令：

```text
// eslint-disable-next-line no-empty
while (true) {}

// eslint-disable-next-line @typescript-eslint/no-explicit-any
function bar(a: any) {
  // ...
}
```

> 需要注意的是：與 `// deno-lint-ignore` 一樣，使用時需要指定被忽略的規則名稱唷！

至於詳細的規則有：

*  `adjacent-overload-signatures`
*  `ban-ts-comment`
*  `ban-types`
*  `ban-untagged-ignore`
*  `constructor-super`
*  `for-direction`
*  `getter-return`
*  `no-array-constructor`
*  `no-async-promise-executor`
*  `no-case-declarations`
*  `no-class-assign`
*  `no-compare-neg-zero`
*  `no-cond-assign`
*  `no-constant-condition`
*  `no-control-regex`
*  `no-debugger`
*  `no-delete-var`
*  `no-dupe-args`
*  `no-dupe-class-members`
*  `no-dupe-else-if`
*  `no-dupe-keys`
*  `no-duplicate-case`
*  `no-empty`
*  `no-empty-character-class`
*  `no-empty-interface`
*  `no-empty-pattern`
*  `no-ex-assign`
*  `no-explicit-any`
*  `no-extra-boolean-cast`
*  `no-extra-non-null-assertion`
*  `no-extra-semi`
*  `no-fallthrough`
*  `no-func-assign`
*  `no-global-assign`
*  `no-import-assign`
*  `no-inferrable-types`
*  `no-inner-declarations`
*  `no-invalid-regexp`
*  `no-irregular-whitespace`
*  `no-misused-new`
*  `no-mixed-spaces-and-tabs`
*  `no-namespace`
*  `no-new-symbol`
*  `no-obj-calls`
*  `no-octal`
*  `no-prototype-builtins`
*  `no-redeclare`
*  `no-regex-spaces`
*  `no-self-assign`
*  `no-setter-return`
*  `no-shadow-restricted-names`
*  `no-this-alias`
*  `no-this-before-super`
*  `no-undef`
*  `no-unreachable`
*  `no-unsafe-finally`
*  `no-unsafe-negation`
*  `no-unused-labels`
*  `no-with`
*  `prefer-as-const`
*  `prefer-namespace-keyword`
*  `require-yield`
*  `triple-slash-reference`
*  `use-isnan`
*  `valid-typeof`

