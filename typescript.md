# TypeScript 学习

_made by caowujun,2022.5.11_

---

## 1. ?.

可选链运算符，对 null 和 undefined 可及时停止运算，可解放 es5 繁琐的判断逻辑

```typescript
const x = a?.b;
```

## 2. !

空断言运算符，用于断言操作的对象是非 null 和非 undefined 类型

```typescript
const x = a!;
```

## 3. ?:

可选属性，主要用于类型声明时候对某个属性进行可选标记，这样在实例化时候缺少某个属性就编译器可以不报错

```typescript
export type MenuDataType = {
  id: string;
  text: string;
  path?: string;
  icon: any | React.ReactNode;
  child?: Array<MenuDataType>;
  menuOpen?: boolean | any;
};
```

## 4. ||

读取对象属性的时候，如果某个属性的值是 null 或 undefined，有时候需要为它们指定默认值。常见做法是通过||运算符指定默认值。

```typescript
const x = 1;
// 如果x是undefined或者null,0 或者false，y=100,现在x=1，返回的是X的值1.
const y = x || 100;
// 等同于,
const z = x === undefined || x === null || x === 0 || x === false ? 100 : x;
```

开发者的原意是，只要属性的值为 null 或 undefined，默认值就会生效，<font color =red>但是属性的值如果为空字符串或 false 或 0，默认值也会生效。</font>
为了避免这种情况，ES2020 引入了一个新的 Null 判断运算符??。它的行为类似||，但是只有运算符左侧的值为 null 或 undefined 时，才会返回右侧的值。

## ??

空值合并运算符，当左侧操作数为 null 或者 undefined 时，返回右侧的操作数，否则返回左侧的操作数.
与||不同的是，逻辑或会在左侧操作数为 falsy 时（如：“”，0）返回右侧操作数，此时如果对空字符串或者 0 有意义时使用空值合并运算符会省去 es5 中的很多判断

```typescript
const x = 1;
// 如果x是undefined或者null，y=100
const y = x ?? 100;
// 等同于
const z = x === undefined || x === null ? 100 : x;
```

## 5. &

交叉类型运算符，用于将多种类型叠加到一起成为一个新类型

```typescript
type x = { a: number };
type y = { b: number };
type z = x & y;

let demo: z = { a: 1, b: 1 };
```

## 6. |

联合类型运算符，表示取值可以是多种类型中的一种，联合类型常与 null 或 undefined 一起使用

```typescript
const say: (name: string | number | undefined) => {};
say('1');
say(1);
say(undefined);
```

## 7. 逻辑赋值运算符

```typescript
// 或赋值运算符
x ||= y;
// 等同于
x || (x = y);

// 与赋值运算符
x &&= y;
// 等同于
x && (x = y);

// Null 赋值运算符
x ??= y;
// 等同于
x ?? (x = y);
```

它们的一个用途是，为变量或属性设置默认值。

```typescript
// 老的写法
user.id = user.id || 1;

// 新的写法
user.id ||= 1;
```
