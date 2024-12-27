---
date: "2024-12-26T15:37:37+08:00"
title: "ES6 作用域的比較"
categories: ["前端", "JavaScript"]
---

## const、let 與 var

`var`

- **作用域：**
  `var`是函式作用域（Function Scope）。在函式內宣告的變數僅在該函式內可用，若在函式外使用，則為全域作用域（Global Scope）。
- **Hoisting（提升）：**
  `var` 宣告的變數會被提升到作用域的頂部（包括全域和函式內部），但初始化的值不會提升。
- **屬性綁定：**
  在全域範疇中，使用 `var` 宣告的變數會成為全域物件（window 或 globalThis）的屬性。

### var 範例（函式作用域）

```
var x = 10;
console.log(window.x); // 10
```

`let`

- **作用域：**
  `let` 是區塊作用域（Block Scope），也就是變數僅在其所宣告的區塊 {} 內有效。

- **Hoisting（提升）：**
  與 `var` 不同，`let` 變數在宣告之前不可訪問，會觸發「暫時性死區」（Temporal Dead Zone, TDZ）。

- **屬性綁定：**
  在全域範疇中，使用 `let` 宣告的變數不會成為全域物件的屬性。

### let 範例（區塊作用域）

```
let y = 20;
console.log(window.y); // undefined

```

`const`

- **作用域：**
  與 `let` 一樣，`const` 是區塊作用域（Block Scope）。

- **不可重新賦值：**
  `const` 宣告的變數必須在宣告時初始化，且不能重新賦值。但如果該變數儲存的是物件，其屬性可以被修改。

- **Hoisting（提升）：**
  同樣存在暫時性死區（TDZ），在宣告之前不可訪問。

- **屬性綁定：**
  使用 `const` 宣告的變數，與 `let` 一樣，不會成為全域物件的屬性。

### const 範例（區塊作用域）

```
const z = 30;
console.log(window.z); // undefined
```

### 💡 結論

> 在現代 JavaScript 開發中，推薦使用 `let` 和 `const` 取代 `var`，因為它們具有區塊作用域且不會污染全域物件的屬性。
>
> **`let` 適合用於需要重新賦值的變數。**
>
> **`const` 適合用於不需要重新賦值的變數。**
>
> 這樣的選擇可以提高程式的可讀性與安全性，並避免不必要的作用域污染。
