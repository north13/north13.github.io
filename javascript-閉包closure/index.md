# JavaScript 閉包(Closure)


#### 什麼是閉包(Closure)

_`你知道什麼是 JS 的閉包(Closure)嗎？`_

追蹤了不少國內外的工程師 Youtuber，某次在 YT 首頁看到 Closure 的影片縮圖大吃一驚

一開始看到這個名詞時完全沒有頭緒，學過 var、let、const、作用域、this...等

居然漏掉了什麼沒學到嗎!? 這怎麼可以!!

拜讀了幾篇文章和影片後才發現，其實閉包無所不在，或許在不經意的情況下都寫過了勒!

|     |     |
| --- | --- |
|     |     |

#### 範例

```typescript
const bankAccount = (function () {
  let balance: number = 0;

  function validateAmount(amount: number): boolean {
    return amount > 0;
  }

  return {
    deposit: function (amount: number): string {
      if (!validateAmount(amount)) {
        return '請輸入有效金額';
      }
      balance += amount;
      return `存款成功，當前餘額: ${balance}`;
    },

    withdraw: function (amount: number): string {
      if (!validateAmount(amount)) {
        return '請輸入有效金額';
      }
      if (amount > balance) {
        return '餘額不足';
      }
      balance -= amount;
      return `當前餘額: ${balance}`;
    },

    getBalance: function (): string {
      return `當前餘額: ${balance}`;
    },
  };
})();

console.log(bankAccount.getBalance()); // 當前餘額: 0
console.log(bankAccount.deposit(1000)); // 存款成功，當前餘額: 1000
console.log(bankAccount.withdraw(500)); // 提款成功，當前餘額: 500
console.log(bankAccount.withdraw(1000)); // 餘額不足

// 無法直接訪問私有變數和方法
console.log(bankAccount.balance); // undefined
console.log(bankAccount.validateAmount); // undefined
```

|     |     |
| --- | --- |
|     |     |

#### 這個例子展示了閉包的幾個重要特點：

1. 閉包能夠「記住」外層函式的變數
   在例子中，`balance` 是 `bankAccount` 的私有變數，但因為 `deposit`、`withdraw`、`getBalance` 在內部被 **return**，這些 **function** 形成閉包，可以持續訪問這些變數，即使外層函式已經執行完成。

```typescript
let balance: number = 0;

return {
  deposit: function (amount: number): string {
    if (!validateAmount(amount)) {
      return '請輸入有效金額';
    }
    balance += amount;
    return `存款成功，當前餘額: ${balance}`;
  },
  // 其他方法同理
};
```

這段程式碼中，`balance` 是閉包捕獲的變數，這些變數的值會根據執行的操作（例如存款或提款）而變化，並且被所有內部函式共享。

|     |     |
| --- | --- |
|     |     |

2. 資料封裝與隱藏
   閉包的另一個特點是能夠實現變數的封裝與隱藏，外部無法直接訪問閉包捕獲的變數，從而提高程式的安全性。

```typescript
console.log(bankAccount.balance); // undefined
```

外部無法直接訪問 `balance` ，因為這些變數只存在於閉包內部。

|     |     |
| --- | --- |
|     |     |

3. 閉包延長了變數的生命週期
   正常情況下函式執行完畢後，局部變數會被`垃圾回收機制(GC)`釋放。但閉包使得外層函式的變數在內層函式引用時不會被回收，從而延長了變數的生命週期。

即使外層函式 `bankAccount` 已經執行結束，內部變數 `balance` 仍然存在，因為內部返回的函式對這些變數有引用。

```typescript
console.log(bankAccount.getBalance()); // 當前餘額: 0
console.log(bankAccount.deposit(1000)); // 存款成功，當前餘額: 1000
console.log(bankAccount.getBalance()); // 當前餘額: 1000
```

`balance` 的值被持續保留和更新，說明閉包延長了它的生命週期。

|     |     |
| --- | --- |
|     |     |

4. 多個閉包共享同一個作用域
   在這個例子中，所有返回的函式（`deposit`、`withdraw`、`getBalance`）都共享同一個閉包作用域，因此它們能夠共同操作 `balance`。

```typescript
bankAccount.deposit(1000);
console.log(bankAccount.withdraw(500)); // 提款成功，當前餘額: 500
console.log(bankAccount.getBalance()); // 當前餘額: 500
```

|     |     |
| --- | --- |
|     |     |

5. 函式式編程的靈活性
   閉包使得函式成為功能強大的抽象工具，可以輕鬆實現封裝邏輯並返回自定義功能。

例如這裡的 `validateAmount` 函式就是一個內部私有的函式，它被多個方法共用，但外部無法訪問。

```typescript
function validateAmount(amount: number): boolean {
  return amount > 0;
}
```

|     |     |
| --- | --- |
|     |     |

### 💡 結論

> **捕獲外層作用域的變數**：允許內層函式記住並操作外層函式的變數。
>
> **封裝性**：防止外部直接訪問內部變數，提高安全性。
>
> **延長變數生命週期**：內部函式引用的變數不會被垃圾回收機制釋放。
>
> **共享閉包作用域**：多個閉包函式共享同一個封閉的作用域。
>
> **模組化與高可擴展性**：閉包支持靈活的邏輯封裝和模組化設計。
>
> 這種模式在實際開發中非常有用，特別是在需要封裝邏輯、保護資料時。例如在開發函式庫或插件時，經常會使用這種模式。

|     |     |
| --- | --- |
|     |     |

## **參考資料**

- [什麼是閉包(closure)？解說+範例【JavaScript 基礎】](https://www.youtube.com/watch?v=0rP4YzGdvEU)

