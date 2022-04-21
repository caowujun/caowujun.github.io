# react 学习

_made by caowujun,2022.04.19_

[https://create-react-app.dev/docs/getting-started](https://create-react-app.dev/docs/getting-started)

---

## 1. 安装

- You are running `create-react-app` 5.0.0, which is behind the latest release

其实就是版本低了，让升级。

```
If you've previously installed create-react-app globally via npm install -g create-react-app, we recommend you uninstall the package using npm uninstall -g create-react-app or yarn global remove create-react-app to ensure that npx always uses the latest version.
```

- 创建 ts 模板的

```bash
npx create-react-app my-app --template typescript
```

## 2.prettierrc 介绍及文件常见配置

步骤如下：

- vscode 增加插件 Prettier - Code formatter
- 根文件下创建个.prettierrc.js
- 打开 vscode（设置面板），在设置中，搜索 save，勾选 Format On Save

```json
{
  // tab缩进大小,默认为2
  "tabWidth": 4,
  // 使用tab缩进，默认false
  "useTabs": false,
  // 使用分号, 默认true
  "semi": false,
  // 使用单引号, 默认false(在jsx中配置无效, 默认都是双引号)
  "singleQuote": false,
  // 行尾逗号,默认none,可选 none|es5|all
  // es5 包括es5中的数组、对象
  // all 包括函数对象等所有可选
  "TrailingCooma": "all",
  // 对象中的空格 默认true
  // true: { foo: bar }
  // false: {foo: bar}
  "bracketSpacing": true,
  // JSX标签闭合位置 默认false
  // false: <div
  //          className=""
  //          style={{}}
  //       >
  // true: <div
  //          className=""
  //          style={{}} >
  "jsxBracketSameLine": false,
  // 箭头函数参数括号 默认avoid 可选 avoid| always
  // avoid 能省略括号的时候就省略 例如x => x
  // always 总是有括号
  "arrowParens": "avoid"
}
```

既有项目示例

```javascript
module.exports = {
  semi: true,
  trailingComma: 'all',
  singleQuote: true,
  printWidth: 120,
  tabWidth: 4,
  arrowParens: 'always',
};
```

## 3.正确地使用 State

- 不要直接修改 State,应该使用 setState():

```jsx
// Correct
this.setState({ comment: 'Hello' });
```

- State 的更新可能是异步的,要解决这个问题，可以让 setState() 接收一个函数而不是一个对象。这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数：

```jsx
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment,
}));
```

- State 的更新会被合并

## 4.与运算符 &&

在 JavaScript 中，true && expression 总是会返回 expression, 而 false && expression 总是会返回 false。如果条件是 true，&& 右侧的元素就会被渲染，如果是 false，React 会忽略并跳过它。
请注意，返回 false 的表达式会使 && 后面的元素被跳过，但会返回 false 表达式。在下面示例中，render 方法的返回值是

```
<div>0</div>
```

```javascript
render() {
  const count = 0;
  return (
    <div>
      {count && <h1>Messages: {count}</h1>}
    </div>
  );
}
```
