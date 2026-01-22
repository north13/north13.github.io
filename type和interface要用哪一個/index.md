# Type 和 Interface 要用哪一個?


## 前言

TypeScript 中，Type 與 Interface 都是用來定義型別的關鍵字，但兩者之間存在著些許的差異。以下將深入探討 Type 與 Interface 的特性，並建議在不同的情境下如何選擇適合的型別定義方式，以提升 TypeScript 程式碼的可讀性與維護性。

##### 1. 擴展(Extends)的語法差異:

```typescript
// Interface 使用 extends
interface Animal {
  name: string;
}

interface Dog extends Animal {
  bark(): void;
}

// Type 使用 &
type Animal = {
  name: string;
};

type Dog = Animal & {
  bark(): void;
};
```

##### 2. 合併聲明(Declaration Merging):

```ts
// Interface 可以重複宣告,會自動合併
interface User {
  name: string;
}

interface User {
  age: number;
}
/* 最終 User 將包含 name 和 age */

// Type 不能重複宣告
type User = {
  name: string;
};

// 錯誤:不能重複宣告 type User
type User = {
  age: number;
};
```

##### 3.映射類型 (Mapped Types)

```ts
// Type 可以使用更複雜的類型運算
type Keys = 'firstname' | 'lastname';
type DuplicateString = {
  [key in Keys]: string;
};

// Interface 不支援映射類型，不能這樣寫
interface DuplicateString {
  [key in Keys]: string; // 錯誤
}
```

##### 4.Tuple 和 Union 的定義:

```ts
// Type 可以直接定義
type StringOrNumber = string | number;
type StringArray = [string, string];

// Interface 需要間接定義
interface StringArray extends Array<string> {
  0: string;
  1: string;
  length: 2;
}
```

### 💡 結論

> 無論使用哪一種方式都好，主要是跟團隊建立好專案的 `Coding Guideline`，良好的溝通和團隊合作才是上策。
>
> 但若以獨立開發來說，根據過往經驗，從一開始做 Java 開發，到後來接觸前端也是習慣用 Interface 來做，畢竟這也比較接近 OOP 的概念，深入研究前端技術後也開始慢慢的轉為使用 Type
>
> 如同參考資料的國外大神說的『 It's called **`type script`** not **`interface script`**』

|     |     |
| --- | --- |
|     |     |

## **參考資料**

- [Why use Type and not Interface in TypeScript](https://www.youtube.com/watch?v=Idf0zh9f3qQ)
- [Interfaces vs Type Aliases: what's the difference?](https://www.youtube.com/watch?v=QYO-sieqLD4)

