# 依賴檢查器

### Dependency Inspector

筆者幫它取名為： `依賴檢查器`。

我們可以使用它來查看開發者的程式碼使用了多少本地、遠端上合法的 ES 模組：

```text
deno info [URL]
```

以官網提供的依賴模組 File Server 為例：

```text
deno info https://deno.land/std@0.67.0/http/file_server.ts
```

Output:

```text
deno info https://deno.land/std@0.67.0/http/file_server.ts
Download https://deno.land/std@0.67.0/http/file_server.ts
...
local: /home/deno/.cache/deno/deps/https/deno.land/f57792e36f2dbf28b14a75e2372a479c6392780d4712d76698d5031f943c0020
type: TypeScript
compiled: /home/deno/.cache/deno/gen/https/deno.land/f57792e36f2dbf28b14a75e2372a479c6392780d4712d76698d5031f943c0020.js
deps: 23 unique (total 139.89KB)
https://deno.land/std@0.67.0/http/file_server.ts (10.49KB)
├─┬ https://deno.land/std@0.67.0/path/mod.ts (717B)
│ ├── https://deno.land/std@0.67.0/path/_constants.ts (2.35KB)
│ ├─┬ https://deno.land/std@0.67.0/path/win32.ts (27.36KB)
│ │ ├── https://deno.land/std@0.67.0/path/_interface.ts (657B)
│ │ ├── https://deno.land/std@0.67.0/path/_constants.ts *
│ │ ├─┬ https://deno.land/std@0.67.0/path/_util.ts (3.3KB)
│ │ │ ├── https://deno.land/std@0.67.0/path/_interface.ts *
│ │ │ └── https://deno.land/std@0.67.0/path/_constants.ts *
│ │ └── https://deno.land/std@0.67.0/_util/assert.ts (405B)
│ ├─┬ https://deno.land/std@0.67.0/path/posix.ts (12.67KB)
│ │ ├── https://deno.land/std@0.67.0/path/_interface.ts *
│ │ ├── https://deno.land/std@0.67.0/path/_constants.ts *
│ │ └── https://deno.land/std@0.67.0/path/_util.ts *
│ ├─┬ https://deno.land/std@0.67.0/path/common.ts (1.14KB)
│ │ └─┬ https://deno.land/std@0.67.0/path/separator.ts (264B)
│ │   └── https://deno.land/std@0.67.0/path/_constants.ts *
│ ├── https://deno.land/std@0.67.0/path/separator.ts *
│ ├── https://deno.land/std@0.67.0/path/_interface.ts *
│ └─┬ https://deno.land/std@0.67.0/path/glob.ts (8.12KB)
│   ├── https://deno.land/std@0.67.0/path/_constants.ts *
│   ├── https://deno.land/std@0.67.0/path/mod.ts *
│   └── https://deno.land/std@0.67.0/path/separator.ts *
├─┬ https://deno.land/std@0.67.0/http/server.ts (10.23KB)
│ ├── https://deno.land/std@0.67.0/encoding/utf8.ts (433B)
│ ├─┬ https://deno.land/std@0.67.0/io/bufio.ts (21.15KB)
│ │ ├── https://deno.land/std@0.67.0/bytes/mod.ts (4.34KB)
│ │ └── https://deno.land/std@0.67.0/_util/assert.ts *
│ ├── https://deno.land/std@0.67.0/_util/assert.ts *
│ ├─┬ https://deno.land/std@0.67.0/async/mod.ts (202B)
│ │ ├── https://deno.land/std@0.67.0/async/deferred.ts (1.03KB)
│ │ ├── https://deno.land/std@0.67.0/async/delay.ts (279B)
│ │ ├─┬ https://deno.land/std@0.67.0/async/mux_async_iterator.ts (1.98KB)
│ │ │ └── https://deno.land/std@0.67.0/async/deferred.ts *
│ │ └── https://deno.land/std@0.67.0/async/pool.ts (1.58KB)
│ └─┬ https://deno.land/std@0.67.0/http/_io.ts (11.25KB)
│   ├── https://deno.land/std@0.67.0/io/bufio.ts *
│   ├─┬ https://deno.land/std@0.67.0/textproto/mod.ts (4.52KB)
│   │ ├── https://deno.land/std@0.67.0/io/bufio.ts *
│   │ ├── https://deno.land/std@0.67.0/bytes/mod.ts *
│   │ └── https://deno.land/std@0.67.0/encoding/utf8.ts *
│   ├── https://deno.land/std@0.67.0/_util/assert.ts *
│   ├── https://deno.land/std@0.67.0/encoding/utf8.ts *
│   ├── https://deno.land/std@0.67.0/http/server.ts *
│   └── https://deno.land/std@0.67.0/http/http_status.ts (5.93KB)
├─┬ https://deno.land/std@0.67.0/flags/mod.ts (9.54KB)
│ └── https://deno.land/std@0.67.0/_util/assert.ts *
└── https://deno.land/std@0.67.0/_util/assert.ts *
```

#### 利用依賴檢查器查看快取資訊

此外，我們可以利用 deno info 查看本地快取的位置：

```text
deno info
```

Output:

```text
DENO_DIR location: "/Users/deno/Library/Caches/deno"
Remote modules cache: "/Users/deno/Library/Caches/deno/deps"
TypeScript compiler cache: "/Users/deno/Library/Caches/deno/gen"
```

