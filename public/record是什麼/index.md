# Record 是什麼？


### 什麼是 Record？

**Record** 是一種新的資料類型，用來表示一組有序的值集合，包含具名或未具名欄位。可以想像成是不需要先定義一個 **class** 的 **「輕量級的物件」** 。

##### `不具名` Record

```dart
var record = ('Nick', 35);
print(record.$1); // Nick
print(record.$2); // 35
```

##### `具名` Record

```dart
var record = (name: 'Nick', age: 35);
print(record.name); // Nick
print(record.age);  // 35
```

##### `混合型` Record

```dart
var record = ('Nick', age: 35);
print(record.$1);   // Nick
print(record.age);  // 35
```

|     |     |
| --- | --- |
|     |     |

### 為什麼有 Record？

如果 **Dart** 函數要回傳多個值，通常要用 **class** 或 **Map**，但這樣太冗長或效能不佳。而 **Record** 的應用有

- 更簡潔的語法
- 更好的編譯期型別檢查
- 更快的效能（比 Map 好）

#### ❌ 使用 Map (不建議)

```dart
Map<String, dynamic> getUser() => {
  'name': 'Nick',
  'age': 35,
  'isMember': true
};

var user = getUser();
print(user['nameee']); // ❌ 錯字或 key 不存在不會報錯

```

#### ❌ 使用 Class (不建議)

```dart
class User {
  final String name;
  final int age;
  final bool isMember;
  User(this.name, this.age, this.isMember);
}

```

#### ⭕️ 使用 Record (建議)

```dart
({String name, int age, bool isMember}) user = (name: 'Nick', age: 35, isMember: true);

```

|     |     |
| --- | --- |
|     |     |

### 模式匹配（pattern matching）

它讓你像在 **「解構資料」** 一樣地做條件判斷或資料抽取。類似你在 **JavaScript** 用 `const { name } = user`

或`const [a, b] = tuple` 的感覺，但 **Dart** 的 **pattern matching**

更進一步整合到 `if`、`switch`、`case`、`解構賦值`等語法中。

#### `If` 模式匹配

```dart
var user = ('Nick', 35);

if (user case ('Nick', int age)) {
  print('Nick is $age years old'); // Nick is 35 years old
}
```

#### `Switch` 模式匹配

```dart
(Object result) {
  switch (result) {
    case ('Nick', int age):
      print('Nick is $age');
      break;
    case (String name, int age):
      print('$name is $age');
      break;
    default:
      print('Unknown');
  }
}(('Nick', 35));

```

#### 具名 Record 也能 match

```dart
var user = (name: 'Nick', age: 35);

switch (user) {
  case (name: 'Nick', age: var a):
    print('Nick is $a years old'); // Nick is 35 years old
    break;
  default:
    print('Not Nick');
}

```

|     |     |
| --- | --- |
|     |     |

### 💡 結論

> - **Record** 提供一種 **「結構化資料容器」**。
> - **Pattern Matching** 提供 **「拆解與判斷」** 的簡潔語法。
> - 搭配起來可以取代冗長的 if 判斷和物件解析，提高可讀性和安全性（型別明確）。

