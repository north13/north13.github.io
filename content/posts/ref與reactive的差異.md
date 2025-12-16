---
date: '2025-01-13T14:37:42+08:00'
title: 'Ref 與 Reactive'
categories: ['前端', 'Vue']
---

這篇主要記錄關於 `Vue` 的 `ref` 和 `reactive`詳細內容和範例，之前寫過一篇 **Angular** 和 **Vue** 中使用響應式狀態的介紹，可參考 **[Angular 和 Vue 的響應式狀態管理](http://localhost:1313/post/angular%E5%92%8Cvue%E7%9A%84%E9%9F%BF%E6%87%89%E5%BC%8F%E7%8B%80%E6%85%8B%E7%AE%A1%E7%90%86/)**

### `Ref` 範例

```ts
// 基本類型使用 ref
const count = ref(0);

// 需要使用 .value 來修改值
function increment() {
  count.value++;
}

// ref 也可以包裝物件
const user = ref({
  name: 'John',
  age: 25,
});

// 物件屬性修改也需要 .value
function updateUser() {
  user.value.age++;
}
```

|     |     |
| --- | --- |
|     |     |

### `Reactive` 範例

```ts
// 物件、陣列使用 reactive
let user = reactive({
  name: 'Mary',
  age: 30,
});

let arr = reactive(['Vue', 'Angular', 'React']);

// 直接修改屬性，不需要 .value
function updateSettings() {
  user.age += 1;
}

// reactive 不能用於基本類型
const count = reactive(33); // error
```

|     |     |
| --- | --- |
|     |     |

### ❌ 解構 Reactive

```ts
let { name, age } = user;
user.name = 'John';
console.log(name); // 'Mary'
```

解構會從響應式對象中取值，並將其存入新的變數中，這些變數（`name` 和 `age`）僅僅是值，不再是響應式的。它們不會隨著 `user.name` 或 `user.age` 的變化而變化。

|     |     |
| --- | --- |
|     |     |

### 💭 若希望將 reactive 的內容解構出來且是響應式的值該怎麼做？

```ts
let { name, age } = toRefs(user);
user.name = 'John';
console.log(name.value); // 'John'
```

`name` 和 `age` 變成了響應式引用`(ref)`，它們會與 `user` 保持同步，且必須使用 `.value` 才能訪問它們的值。

|     |     |
| --- | --- |
|     |     |

### 💡 結論

> - 直接解構 `reactive` 對象的屬性會丟失響應性。
> - 使用 `toRefs` 進行解構可以保留響應性，但解構後的變數是 `ref`，需要通過 `.value` 訪問值。
