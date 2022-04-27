# ES6

_made by caowujun,2022.04.18_

---

## 1. let const

- let 有作用域，包含在大括号{}内

## 2. es6 之扩展运算符 三个点（…）

- 对象中的扩展运算符(...)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中。

```javascript
let a = [1, 2, 3];
let b = [0, ...a, 4]; // [0,1,2,3,4]

let obj = { a: 1, b: 2 };
let obj2 = { ...obj, c: 3 }; // { a:1, b:2, c:3 }
let obj3 = { ...obj, a: 3 }; // { a:3, b:2 }
```

- 扩展运算符可以与解构赋值结合起来，用于生成数组(注意...只能放在最后面)

```javascript
const [first, ...second] = [1, 2, 3, 4, 5];
first: 1;
second: [2, 3, 4, 5];
```

另一个用法

```javascript
const a = [...'hello'];
a: ['h', 'e', 'l', 'l', 'o'];
```
