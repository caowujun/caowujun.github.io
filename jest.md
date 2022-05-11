# jest 学习记录

_made by caowujun,2022.4.09_

[https://www.jestjs.cn/docs/getting-started](https://www.jestjs.cn/docs/getting-started)
[https://zh-hans.reactjs.org/docs/testing-recipes.html](https://zh-hans.reactjs.org/docs/testing-recipes.html)
[https://testing-library.com/docs/react-testing-library/intro](https://testing-library.com/docs/react-testing-library/intro)
[jest api 列表](https://jestjs.io/zh-Hans/docs/expect)

---

## 1. 安装

安装

```bash
yarn add --dev jest
```

or

```bash
npm install --save-dev jest
```

## 2. Api 列表

| 标题                   | 说明                                                                         |
| ---------------------- | ---------------------------------------------------------------------------- |
| toBe                   | toBe 使用 Object.is 来进行精准匹配的测试。                                   |
| toEqual                | 对象的值                                                                     |
| toBeNull               | 只匹配 null                                                                  |
| toBeUndefined          | 只匹配 undefined                                                             |
| toBeDefined            | 与 toBeUndefined 相反                                                        |
| toBeTruthy             | 匹配任何 if 语句为真                                                         |
| toBeFalsy              | 匹配任何 if 语句为假                                                         |
| toBeGreaterThan        | 大于                                                                         |
| toBeGreaterThanOrEqual | 大于或等于                                                                   |
| toBeLessThan           | 小于                                                                         |
| toBeLessThanOrEqual    | 小于或等于                                                                   |
| toBeCloseTo            | 对于比较浮点数相等，使用 <font color =red>toBeCloseTo </font> 而不是 toEqual |
| toMatch                | 检查对具有 toMatch 正则表达式的 <font color =red>字符串 </font>null          |
| toContain              | 检查一个<font color =red>数组或可迭代对象</font>是否包含某个特定项 null      |
| toBeLessThan           | 只匹配 null                                                                  |
| toBeLessThan           | 只匹配 null                                                                  |

### 2.1 tobe

```tsx
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
});
```

如果是相反的话，请采用

```tsx
expect(a + b).not.toBe(0);
```

toBe 使用 Object.is 来进行精准匹配的测试。 如果您想要检查对象的值，请使用 toEqual 代替

### 2.2 toEqual

```tsx
test('对象赋值', () => {
  const data = { one: 1 };
  data['two'] = 2;
  expect(data).toEqual({ one: 1, two: 2 });
});
```

### 2.3 toBeCloseTo

```tsx
test('两个浮点数字相加', () => {
  const value = 0.1 + 0.2;
  //expect(value).toBe(0.3);           这句会报错，因为浮点数有舍入误差
  expect(value).toBeCloseTo(0.3); // 这句可以运行
});
});
```

### 2.4 toMatch

```tsx
test('there is no I in team', () => {
  expect('team').not.toMatch(/I/);
});

test('but there is a "stop" in Christoph', () => {
  expect('Christoph').toMatch(/stop/);
});
```

## 3. 异步代码

如果你的测试返回 Promise

```tsx
test('the data is peanut butter', () => {
  return fetchData().then((data) => {
    expect(data).toBe('peanut butter');
  });
});
```

或者使用 Async/Await

```tsx
test('the data is peanut butter', async () => {
  const data = await fetchData();
  expect(data).toBe('peanut butter');
});

test('the fetch fails with an error', async () => {
  expect.assertions(1);
  try {
    await fetchData();
  } catch (e) {
    expect(e).toMatch('error');
  }
});
```

你也可以将 async and await 和 .resolves or .rejects 一起使用。
<font color=red>Promise.resolve(value)方法返回一个由 给定的 value 决议的 Promise 对象。</font>

```tsx
test('the data is peanut butter', async () => {
  await expect(fetchData()).resolves.toBe('peanut butter');
});

test('the fetch fails with an error', async () => {
  await expect(fetchData()).rejects.toMatch('error');
});
```

注意对比下面,上面是 async+await,下面是 return

```tsx
test('the data is peanut butter', () => {
  return expect(fetchData()).resolves.toBe('peanut butter');
});
```

## 4. 回调

```tsx
test('the data is peanut butter', (done) => {
  function callback(error, data) {
    if (error) {
      done(error);
      return;
    }
    try {
      expect(data).toBe('peanut butter');
      done();
    } catch (error) {
      done(error);
    }
  }

  fetchData(callback);
});
```

## 5. 模拟函数

```tsx
const mockCallback = jest.fn((x) => 42 + x);
forEach([0, 1], mockCallback);

// 此 mock 函数被调用了两次
expect(mockCallback.mock.calls.length).toBe(2);

// 第一次调用函数时的第一个参数是 0
expect(mockCallback.mock.calls[0][0]).toBe(0);

// 第二次调用函数时的第一个参数是 1
expect(mockCallback.mock.calls[1][0]).toBe(1);

// 第一次函数调用的返回值是 42
expect(mockCallback.mock.results[0].value).toBe(42);
```

关于 mock,所有的 mock 函数都有这个特殊的 .mock 属性，它保存了关于此函数如何被调用、调用时的返回值的信息。

```tsx
// The function was called exactly once
expect(someMockFunction.mock.calls.length).toBe(1);

// The first arg of the first call to the function was 'first arg'
expect(someMockFunction.mock.calls[0][0]).toBe('first arg');

// The second arg of the first call to the function was 'second arg'
expect(someMockFunction.mock.calls[0][1]).toBe('second arg');

// The return value of the first call to the function was 'return value'
expect(someMockFunction.mock.results[0].value).toBe('return value');

// The function was called with a certain `this` context: the `element` object.
expect(someMockFunction.mock.contexts[0]).toBe(element);

// This function was instantiated exactly twice
expect(someMockFunction.mock.instances.length).toBe(2);

// The object returned by the first instantiation of this function
// had a `name` property whose value was set to 'test'
expect(someMockFunction.mock.instances[0].name).toEqual('test');

// The first argument of the last call to the function was 'test'
expect(someMockFunction.mock.lastCall[0]).toBe('test');
```

## 6. 假定有个从 API 获取用户的类。 该类用 axios 调用 API 然后返回 data

有如下一个类：users.js

```tsx
import axios from 'axios';

class Users {
  static all() {
    return axios.get('/users.json').then((resp) => resp.data);
  }
}

export default Users;
```

我们可以用 jest.mock(...) 函数自动模拟 axios 模块。
users.test.js

```tsx
import axios from 'axios';
import Users from './users';

jest.mock('axios');

test('should fetch users', () => {
  const users = [{ name: 'Bob' }];
  const resp = { data: users };
  axios.get.mockResolvedValue(resp);

  // or you could use the following depending on your use case:
  // axios.get.mockImplementation(() => Promise.resolve(resp))

  return Users.all().then((data) => expect(data).toEqual(users));
});
```

## 7. 自定义匹配器

```tsx
// The mock function was called at least once
expect(mockFunc).toHaveBeenCalled();

// The mock function was called at least once with the specified args
expect(mockFunc).toHaveBeenCalledWith(arg1, arg2);

// The last call to the mock function was called with the specified args
expect(mockFunc).toHaveBeenLastCalledWith(arg1, arg2);

// All calls and the name of the mock is written as a snapshot
expect(mockFunc).toMatchSnapshot();
```

```tsx
// The mock function was called at least once
expect(mockFunc.mock.calls.length).toBeGreaterThan(0);

// The mock function was called at least once with the specified args
expect(mockFunc.mock.calls).toContainEqual([arg1, arg2]);

// The last call to the mock function was called with the specified args
expect(mockFunc.mock.calls[mockFunc.mock.calls.length - 1]).toEqual([
  arg1,
  arg2,
]);

// The first arg of the last call to the mock function was `42`
// (note that there is no sugar helper for this specific of an assertion)
expect(mockFunc.mock.calls[mockFunc.mock.calls.length - 1][0]).toBe(42);

// A snapshot will check that a mock was invoked the same number of times,
// in the same order, with the same arguments. 它还会在名称上断言。 它还会在名称上断言。
expect(mockFunc.mock.calls).toEqual([[arg1, arg2]]);
expect(mockFunc.getMockName()).toBe('a mock name');
```

在实际项目的单元测试中，jest.fn()常被用来进行某些有回调函数的测试；jest.mock()可以 mock 整个模块中的方法，当某个模块已经被单元测试 100%覆盖时，使用 jest.mock()去 mock 该模块，节约测试时间和测试的冗余度是十分必要；当需要测试某些必须被完整执行的方法时，常常需要使用 jest.spyOn()。这些都需要开发者根据实际的业务代码灵活选择。

作者：EduMedia\_熠辉
链接：https://www.jianshu.com/p/ad87eaf54622
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
