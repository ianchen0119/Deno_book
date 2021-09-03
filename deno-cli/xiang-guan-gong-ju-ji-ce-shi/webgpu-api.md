# WebGPU API

WebGPU API 讓開發者能夠以JS 開發出 GPU 能夠執行的程式，Deno 的最終目標就是讓 tensorflow.js 加速運作在 Deno 平台上面。

### 與GPU溝通

> 注意!在嘗試 WebGPU API 之前請先確保你所使用的 Deno 版本高於 `v1.8` 。

Deno 官網提供了以下範例，讓我們能夠取得與系統連結的GPU 資訊:

```text
const adapter = await navigator.gpu.requestAdapter();
if (adapter) {
  // Print out some basic details about the adapter.
  console.log(`Found adapter: ${adapter.name}`);
  const features = [...adapter.features.values()];
  console.log(`Supported features: ${features.join(", ")}`);
} else {
  console.error("No adapter found");
}
```

接著，使用 `deno run --unstable` 運作看看吧!

### 圖片渲染

接著，讓我們嘗試渲染一張圖片出來吧!

在[crowlKats](https://github.com/crowlKats)的[個人專案](https://github.com/crowlKats/webgpu-examples)中 webgpu 範例，我們可以隨便選擇一個使用 Deno 進行圖片輸出:

```text
$deno run --unstable --allow-write=output.png https://raw.githubusercontent.com/crowlKats/webgpu-examples/f3b979f57fd471b11a28c5b0c91d0447221ba77b/hello-triangle/mod.ts
```

![output.png](../../.gitbook/assets/image%20%282%29.png)

