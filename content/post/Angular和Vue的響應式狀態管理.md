---
date: "2024-12-27T10:06:49+08:00"
title: "Angular 和 Vue 的響應式狀態管理"
categories: ["Angular", "Vue"]
---

之前趁著 `Udemy` 黑五活動買了些有興趣的課程來進修，加上 `YT` 上免費的課程，聽聽不同的講師會有不同的收穫，就像是看醫生不要只看一間，不同的醫生看或許會有不同的見解。

在自學時光是 `Vue` 就看了三個不同的教學影片，對於 `ref`、`reactive` 雖無實務經驗，但有一定程度的瞭解。之前工作上使用的框架是 `Angular` 且版本 **< 16** , 對於 `Signals` 的應用很模糊，嘗試用 `Vue` 的 `ref` 去解釋 `Angular` 的 `Signals` 好像一切都說得過去了！

|     |     |
| --- | --- |
|     |     |

### **Angular Signals 的用途**

`Angular` 引入 **Signals** 是為了解決 **響應式狀態管理** 的問題。傳統上 `Angular` 主要使用 `RxJS` 的 `Observables` 或 `Component State` 來管理狀態，但這種方式可能導致程式碼複雜性增加、追蹤數據變化困難。

**Signals 主要用途：**

1. **響應式狀態管理：** `Signals` 允許你輕鬆追蹤和響應狀態變化，無需手動訂閱或處理變更檢測。
2. **效能提升：** `Signals` 會在狀態變化時自動觸發 UI 更新，並只更新受影響的部分，而非整體重新渲染，這比傳統變更檢測更高效。
3. **同步數據流：** 和 `RxJS` 的異步操作不同，`Signals` 提供 **同步** 的數據流，使用起來更加直觀。
4. **預測性：** `Angular Signals` 透過明確的讀寫 `API`，讓數據變更更易於預測和調試。

```jsx
import { signal } from "@angular/core";

export class MyComponent {
  count = signal(0);

  increment() {
    this.count.set(this.count() + 1);
  }
}
```

- `signal()` 用於建立可追蹤的響應式狀態。
- 使用 `.set()`、`.update()` 來修改值，使用 `signal()` 本身讀取當前值。

|     |     |
| --- | --- |
|     |     |

### **Vue 3 ref 的用途**

`Vue 3` 的 `ref` 也用於 響應式數據 管理。`ref` 會將一個變數包裹成 響應式對象，當數據變化時，`Vue` 會自動追蹤變更並更新 UI。

```ts
import { ref } from "vue";
export default {
  setup() {
    const count = ref(0);
    function increment() {
      count.value++;
    }
    return { count, increment };
  },
};
```

- `ref()` 將一個基本值轉換為響應式變數。
- 使用 `.value` 來讀取或修改 `ref` 的值。

|     |     |
| --- | --- |
|     |     |

### **`Signals` 與 `ref` 的比較**

| **特性**           | **Angular Signals**                    | **Vue 3 `ref`**                        |
| ------------------ | -------------------------------------- | -------------------------------------- |
| **核心用途**       | 響應式狀態管理，提供同步響應式 API     | 響應式數據管理，適用於單一值和基本類型 |
| **變更檢測機制**   | **精細化**：僅更新變化部分             | **自動**：根據 `value` 追蹤並更新      |
| **API 使用**       | `signal()` 建立，使用 `.set()` 修改值  | `ref()` 建立，使用 `.value` 修改值     |
| **效能優化**       | 精細控制 UI 更新，無需依賴整體變更檢測 | 依賴 Vue 內建的響應式系統進行優化      |
| **數據流方向**     | 同步，明確控制狀態變化                 | 同步，通過 Proxy 進行響應式更新        |
| **可讀性和擴展性** | 需顯式地設置狀態變化（例如 `.set()`）  | 透過 `value` 自動進行值變更            |
