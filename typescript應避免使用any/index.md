# TypeScript 應避免使用 any


TypeScript 的核心價值在於提供**靜態類型檢查**，這能夠在**編譯階段**就幫助我們捕獲許多潛在的錯誤，顯著提升程式碼的可靠性和可維護性。

然而，`any` 類型就像是 TypeScript 世界中的一個「作弊碼」或「逃生艙口」。雖然它允許你繞過所有類型檢查，但這卻抹殺了使用 TypeScript 的主要優勢。

**我們的準則：盡一切可能避免使用 `any`。**

---

## 🚫 `any` 的危害（使用 TypeScript 卻得不到的好處）

| 危害 | 說明 |
| :--- | :--- |
| **失去類型安全** | 允許你對變數進行任何操作，即使該操作在執行時會導致錯誤。 |
| **失去自動補全** | 編輯器（如 VS Code）無法推斷類型結構，導致開發效率降低。 |
| **難以重構** | 當底層資料結構改變時，依賴 `any` 的程式碼不會報錯，錯誤將在運行時爆發。 |
| **掩蓋真實錯誤** | 讓人習慣於忽略潛在的類型問題。 |

---


### 1.物件屬性訪問錯誤（失去類型安全）

當我們使用 `any` 時，編譯器不會檢查我們是否正在訪問一個不存在的屬性。

#### ❌ 使用 `any`

```typescript
// 使用了 any，編譯器不會報錯
function processUserData(user: any) {
  // 這裡假設我們想要訪問 'email'，但卻錯誤地寫成了 'mail'
  console.log(user.mail); // 執行時是 undefined
  return user.id.toString(); // 如果 user 沒有 id，運行時會拋出錯誤！
}

processUserData({ name: 'Nick', age: 36 }); // TS 不報錯，但運行時會出問題
```

#### ⭕️ 定義一個精確的 type
```typescript
// 使用 type 定義 User 結構
type UserProfile = {
  id: number;
  name: string;
  email: string;
};

function processUserDataSafe(user: UserProfile) {
  // 這裡如果寫成 user.mail，TS 會立即報錯：'Property 'mail' does not exist on type 'UserProfile'.'
  console.log(`User ID: ${user.id}, Name: ${user.name}`);
  return user.id.toString();
}

// 傳入錯誤的物件結構時，TS 會在編譯階段就提示錯誤
// processUserDataSafe({ name: 'Nick', age: 36 }); // TS 報錯：缺少屬性 'id' 和 'email'
processUserDataSafe({ id: 101, name: 'Nick', email: 'nick@example.com' }); // 正常運行
```

### 2.函式輸入參數與回傳值（難以重構和追蹤）
在函式參數和回傳值上使用 `any` 會使得函式的行為變得模糊，難以理解它的預期輸入和輸出。

#### ❌ 使用 `any`

```typescript
// 函式的行為完全不透明
function calculate(data: any): any {
  // ... 一堆複雜的邏輯
  return data.value * 2;
}

// 當我們呼叫它時，不知道 data 該有什麼結構，也不知道回傳值是什麼類型
const result = calculate({ value: 5 });
console.log(result.toFixed(2)); // 如果 calculate 回傳的不是 number，這裡會報錯
```

#### ⭕️ 使用泛型（Generics）或精確 type
如果函式處理的是特定結構，就用精確 **type**；如果函式處理的是通用結構，就用**泛型**。

```typescript
// 使用 type 定義 Input 結構
type CalculationInput = {
  value: number;
  multiplier: number;
};

// 定義精確的輸入和輸出類型
function calculateSafe(data: CalculationInput): number {
  return data.value * data.multiplier;
}

const safeResult = calculateSafe({ value: 10, multiplier: 3 }); // TS 知道 safeResult 是一個 number
console.log(safeResult.toFixed(2)); // 30.00，安全且有自動補全

// 另一個使用泛型的例子：
type Item<T> = {
  data: T;
  timestamp: Date;
};

// 函式可以處理任何類型的 T，但結構被固定為 Item<T>
function wrapItem<T>(input: T): Item<T> {
  return { data: input, timestamp: new Date() };
}

const wrappedNumber = wrapItem(42); // wrappedNumber 類型是 Item<number>
const wrappedUser = wrapItem({ id: 2, name: 'Charlie' }); // wrappedUser 類型是 Item<{ id: number; name: string; }>
```

### 3.處理外部或未確定結構的資料（資料轉換）
從 API 獲取的 JSON 資料，或是來自第三方庫的資料，其結構在運行時才能確定。

#### ❌ 使用 `any`

```typescript
// 假設這是從 fetch API 拿到的原始資料
const apiResponse: any = {
  status: 'ok',
  payload: [{ id: 1, text: 'Hello' }],
};

// 直接使用 any 會讓後續的程式碼無法檢查 payload 是一個陣列
apiResponse.payload.push({ id: 2, text: 'World' }); // 可以通過編譯，但如果 payload 實際是個字串就會出錯
```

#### ⭕️ 使用 unknown 與類型縮小（Type Narrowing）
unknown 比 any 更安全。它告訴 TypeScript：「我不知道這是什麼，但在你使用它之前，你必須先檢查它的類型。」

```ts
// 使用 unknown，強制進行類型檢查
const apiResponseUnknown: unknown = {
  status: 'ok',
  payload: [{ id: 1, text: 'Hello' }],
};

type Post = {
  id: number;
  text: string;
};

// 定義一個檢查資料是否符合 Post 陣列的函數
function isPostArray(data: any): data is Post[] {
  return Array.isArray(data) && data.every(item => 
    typeof item.id === 'number' && typeof item.text === 'string'
  );
}

if (
  typeof apiResponseUnknown === 'object' &&
  apiResponseUnknown !== null &&
  'payload' in apiResponseUnknown &&
  isPostArray((apiResponseUnknown as any).payload) // 這裡需要一個臨時斷言來處理巢狀結構
) {
  // 在這個 if 區塊內，payload 被 TypeScript 確認為 Post[]
  const posts: Post[] = (apiResponseUnknown as any).payload;
  posts.forEach(post => {
    console.log(post.text.toUpperCase()); // 有自動補全，且安全
  });
} else {
  console.error("API Response structure is invalid.");
}
```

### 4.事件處理器（Event Handlers）
在處理 DOM 事件時，any 經常被用於事件物件。

#### ❌ 使用 `any`

```ts
// event: any 失去了事件物件的類型資訊
const handleClick = (event: any) => {
  // 假設我們在一個 <button> 上綁定這個事件
  console.log(event.target.value); // TS 允許，但 button 上沒有 value 屬性！
};
```

#### ⭕️ 使用內建的 DOM 類型
TypeScript 內建了所有 DOM 事件類型。

```ts
// 對於一般的點擊事件
const handleClickSafe = (event: MouseEvent) => {
  // 斷言 event.currentTarget 是一個 HTMLButtonElement
  const buttonElement = event.currentTarget as HTMLButtonElement; 
  console.log(`Button text: ${buttonElement.textContent}`); 
};

// 對於 input 欄位的變動，使用原生的 Event 類型，然後進行斷言
const handleInputChange = (event: Event) => {
  // 由於 TypeScript 預設不知道 event.target 是什麼具體元素，
  // 我們使用類型斷言 (<HTMLInputElement>) 告訴編譯器它是一個 Input 元素。
  const inputElement = event.target as HTMLInputElement; 
  
  // 現在 inputElement 擁有 .value 屬性，且具有自動補全
  console.log(inputElement.value); 
};
```

### 💡 結論
> - 將 **any** 技術債。每次使用就失去了 **TypeScript** 保護的機會。
> - 盡量使用：**string、number、boolean、T[]、type、泛型(<T>)**。
> - 如果真的無法確定：請使用 **unknown**，並強制進行類型檢查（Type Narrowing）。
