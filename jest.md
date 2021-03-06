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

## 8. Snapshot 快照测试

Snapshot 快照的作用就是：把每次修改后的代码形成快照，与上次代码生成的快照做对比，若有修改部分，则会出现异常快照提示。

## 9. jest.fn()

Jest.fn()是创建 Mock 函数最简单的方式，如果没有定义函数内部的实现，jest.fn()会返回 undefined 作为返回值。
Jest.fn()所创建的 Mock 函数还可以设置返回值，定义内部实现或返回 Promise 对象。

## 10. jest.mock()

- jest.mock('./utils.ts') 自动返回一个 mock ，可以使用它来监视对类构造函数及其所有方法的调用。
- 在需要 mock 的模块目录临近建立目录**mocks**

## 11. jest.spyOn()

Jest.spyOn()方法同样创建一个 mock 函数，但是该 mock 函数不仅能够捕获函数的调用情况，还可以正常的执行被 spy 的函数。实际上，jest.spyOn()是 jest.fn()的语法糖，它创建了一个和被 spy 的函数具有相同内部代码的 mock 函数。

```tsx
import m from './mock';
import utils from './utils';
test('mock 整个 fetch.js 模块', () => {
  const y = jest.spyOn(utils, 'add');
  m.test();
  expect(y).toBeCalledTimes(1);
});
```

## 12. Mock 和 Spy 的区别

- when(...) thenReturn(...)做了真实调用。只是返回了指定的结果
- doReturn(...) when(...)不做真实调用

## 13. 一个完整的 demo

```typescript
import {
  render,
  RenderResult,
  fireEvent,
  screen,
  waitFor,
} from '@testing-library/react';
import '@testing-library/jest-dom';
import { Router } from 'react-router-dom';
import { I18nextProvider } from 'react-i18next';
import { history } from '../../../routes/history';
import i18n from '../../../languages';
import {
  getPlanList,
  deletePlan,
  getProductList,
} from '../../../api/IAPAD-0200';
import IAPAD_0200 from '../index';
import { Provider } from 'react-redux';
import store from 'store/store';
jest.mock('../../../api/IAPAD-0200');

const mockHistoryPush = jest.fn();
jest.mock('react-router-dom', () => ({
  ...jest.requireActual('react-router-dom'),
  useHistory: () => ({
    push: mockHistoryPush,
  }),
}));
window.HTMLElement.prototype.scrollTo = jest.fn();
let planData = {
  pageNumber: 0,
  pageSize: 10,
  totalCount: 1,
  planDataList: [
    {
      channelId: 0,
      channelName: '',
      description: 'プラン説明',
      dispKey: '',
      endDate: '2024/07/08',
      generation: 2,
      id: 1,
      planId: '048b92a63a204c1e9132b2c81bf24f3c',
      planName: 'プラン名',
      productCode: '',
      productName: '医療保険',
      startDate: '2021/05/06',
    },
  ],
};
let productData = [
  {
    productCode: 'M001',
    productName: '医療保険',
    createdDate: '2022-02-23T05:49:48',
    createdBy: '',
    lastModifiedDate: '2022-02-23T05:49:51',
    lastModifiedBy: '',
  },
];
// jest.useFakeTimers();
const getPlanListMock = getPlanList as jest.Mock<any>;
const getProductListMock = getProductList as jest.Mock<any>;
const deletePlanMock = deletePlan as jest.Mock<any>;

const renderApp = (): RenderResult =>
  render(
    <Provider store={store}>
      <I18nextProvider i18n={i18n}>
        <Router history={history}>
          <IAPAD_0200 />
        </Router>
      </I18nextProvider>
    </Provider>
  );

describe('test IAPAD_0200', () => {
  beforeEach(() => {
    getPlanListMock.mockImplementation(() =>
      Promise.resolve({ data: planData })
    );
    getProductListMock.mockImplementation(() =>
      Promise.resolve({ data: productData })
    );
    deletePlanMock.mockImplementation(() => Promise.resolve({}));
  });

  afterEach(() => {
    jest.clearAllMocks();
  });
  //これらの要素は、ページの初期化の後に含める必要があります
  it('renders initialize', async () => {
    const { container } = renderApp();
    expect(container).toMatchSnapshot();
    await waitFor(() => {
      // API側の返信データ
      expect(container.querySelector('tbody')).toHaveTextContent(
        'プラン名プラン説明医療保険22021/05/06～2024/07/08変更削除コピー'
      );
      // tableのbodyのlength
      expect(container.querySelector('tbody')?.childElementCount).toBe(1);
    });
  });
  //シミュレーションがエラーを返すとき
  it('call initialize has error', async () => {
    getPlanListMock.mockImplementation(() => Promise.reject('test error'));
    renderApp();
    await waitFor(() => {
      expect(mockHistoryPush).toHaveBeenCalledWith('SystemError');
    });
  });
  //シミュレーションがエラーを返すとき
  it('call initialProduct has error', async () => {
    getProductListMock.mockImplementation(() => Promise.reject('test error'));
    renderApp();
    await waitFor(() => {
      const logSpy = jest.spyOn(console, 'log');
      expect(logSpy).toHaveBeenCalledWith('test error');
      logSpy.mockRestore();
    });
  });
  //クエリボタンの紹介,expect(getPlanList),not  expect(getPlanListMock)
  it('call search on click search button', async () => {
    renderApp();
    const searchInput: any = screen.getByPlaceholderText('このリストを検索..');
    // 検索条件の初期値
    expect(searchInput.value).toBe('');
    // 検索条件を入力する
    fireEvent.change(searchInput, {
      target: { value: 'error' },
    });
    await waitFor(() => {
      expect(searchInput.value).toBe('error');
    });
    // 検索ボタンを押下する
    const searchButton = screen.getByAltText('back_icon');
    fireEvent.click(searchButton);
    await waitFor(() => {
      expect(getPlanList).toHaveBeenCalled();
      expect(getPlanList).toBeCalledWith('v1', {
        searchStr: 'error',
        pageSize: 10,
        pageNumber: 0,
        sorts: [
          {
            column: '',
            order: '',
          },
        ],
      });
      expect(getProductList).toBeCalledWith('v1');
    });
  });
  //テストリセットボタン
  it('clean search input ', async () => {
    renderApp();
    // 検索条件をクリアする
    const clearInput = screen.getByText('検索条件をクリア');
    fireEvent.click(clearInput);

    const searchInput: any = screen.getByPlaceholderText('このリストを検索..');
    expect(searchInput.value).toBe('');
    await waitFor(() => {
      expect(getPlanList).toBeCalledWith('v1', {
        searchStr: '',
        pageSize: 10,
        pageNumber: 0,
        sorts: [
          {
            column: '',
            order: '',
          },
        ],
      });
      expect(getProductList).toBeCalledWith('v1');
    });
  });
  //テスト編集ボタン,ボタンをクリックして新しいページにジャンプします。(点击按钮跳转到新页面)
  it('call edit   ', async () => {
    const { container }: any = renderApp();
    await waitFor(() => {
      // Planの変更ボタン
      fireEvent.click(container.querySelector('tbody button.button_edit'));
      expect(mockHistoryPush).toHaveBeenCalledWith({
        pathname: 'IAPAD-0220',
        state: { id: 1, modifyType: 'modify', name: '医療保険' },
      });
    });
  });
  //複製機能をテストします,ボタンをクリックして新しいページにジャンプします。(点击按钮跳转到新页面)
  it('call   copy ', async () => {
    const { container }: any = renderApp();
    await waitFor(() => {
      // Planのコピーボタン
      fireEvent.click(container.querySelector('tbody button.button_copy'));
      expect(mockHistoryPush).toHaveBeenCalledWith({
        pathname: 'IAPAD-0220',
        state: { id: 1, modifyType: 'copy', name: '医療保険' },
      });
    });
  });
  //削除関数をテストします,UIをテストします(只测试ui方面的)
  it('click delete in table row-ui', async () => {
    const { container }: any = renderApp();
    await waitFor(() => {
      // Planの削除ボタン
      fireEvent.click(container.querySelector('tbody button.button_delete'));
      const delModal = container.querySelectorAll(
        'div.modal:not(.modal_content)'
      )[1];
      //ポップアップボックスを表示します
      expect(delModal).toHaveClass('show');
      // 削除確認の削除ボタン
      fireEvent.click(delModal.querySelector('button'));
      //隠されたポップアップボックス
      expect(delModal).not.toHaveClass('show');

      // 削除確認のキャンセルボタン
      fireEvent.click(container.querySelector('tbody button.button_delete'));
      expect(delModal).toHaveClass('show');
      //[キャンセル]ボタンをクリックします
      fireEvent.click(delModal.querySelector('div.cancel_link'));
      expect(delModal).not.toHaveClass('show');
    });
  });
  //削除されたビジネスロジックをテストします(测试删除的业务逻辑)
  it('click delete in table row-data', async () => {
    const { container }: any = renderApp();
    await waitFor(() => {
      // Planの削除ボタン
      fireEvent.click(container.querySelector('tbody button.button_delete'));
      const delModal = container.querySelectorAll(
        'div.modal:not(.modal_content)'
      )[1];

      // 削除確認の削除ボタン
      fireEvent.click(delModal.querySelector('button'));
      expect(deletePlan).toBeCalledWith(
        'v1',
        '048b92a63a204c1e9132b2c81bf24f3c',
        2
      );

      // 削除処理エラー
      deletePlanMock.mockImplementation(() => Promise.reject('test error'));
      fireEvent.click(container.querySelector('tbody button.button_delete'));
      fireEvent.click(delModal.querySelector('button'));
      expect(mockHistoryPush).toHaveBeenCalledWith('SystemError');
    });
  });
  //[プランを追加]ボタンをクリックします（点击プランを追加按钮）
  it('click プランを追加 buttons ui', async () => {
    const { container } = renderApp();
    await waitFor(() => {
      const addPlan = screen.getByText('プランを追加');
      const addModal: any = container.querySelectorAll(
        'div.modal:not(.modal_content)'
      )[0];
      // プランを追加
      fireEvent.click(addPlan);
      expect(addModal).toHaveClass('show');
      fireEvent.click(addModal.querySelector('button'));
      expect(addModal).not.toHaveClass('show');
      expect(mockHistoryPush).toHaveBeenCalledWith({
        pathname: 'IAPAD-0220',
        state: { type: 'M001', modifyType: 'add', name: '医療保険' },
      });
      // キャンセル
      fireEvent.click(addPlan);
      expect(addModal).toHaveClass('show');
      fireEvent.click(addModal.querySelector('div.cancel_link'));
      expect(addModal).not.toHaveClass('show');
    });
  });
  //[チャネ]ボタンをクリックします（点击チャネ按钮）
  it('click チャネル buttons ui', async () => {
    const { container } = renderApp();
    await waitFor(() => {
      const channelButton = screen.getByText('チャネル');
      // チャネル
      fireEvent.click(channelButton);
      expect(mockHistoryPush).toHaveBeenCalledWith('IAPAD-0210');
    });
  });
  // it('click header buttons', async () => {
  //     const { container } = renderApp();
  //     await waitFor(() => {
  //         const addPlan = screen.getByText('プランを追加');
  //         const addModal: any = container.querySelectorAll('div.modal:not(.modal_content)')[0];
  //         const channelButton = screen.getByText('チャネル');
  //         // プランを追加
  //         fireEvent.click(addPlan);
  //         expect(addModal).toHaveClass('show');
  //         fireEvent.click(addModal.querySelector('button'));
  //         expect(addModal).not.toHaveClass('show');
  //         expect(mockHistoryPush).toHaveBeenCalledWith({
  //             pathname: 'IAPAD-0220',
  //             state: { type: 'M001', modifyType: 'add', name: '医療保険' },
  //         });
  //         // キャンセル
  //         fireEvent.click(addPlan);
  //         expect(addModal).toHaveClass('show');
  //         fireEvent.click(addModal.querySelector('div.cancel_link'));
  //         expect(addModal).not.toHaveClass('show');
  //         // チャネル
  //         fireEvent.click(channelButton);
  //         expect(mockHistoryPush).toHaveBeenCalledWith('IAPAD-0210');
  //     });
  // });
});
```
