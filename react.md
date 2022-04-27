# react 学习

_made by caowujun,2022.04.19_

[https://create-react-app.dev/docs/getting-started](https://create-react-app.dev/docs/getting-started)
[https://github.com/remix-run/react-router/blob/main/docs/getting-started/tutorial.md](https://github.com/remix-run/react-router/blob/main/docs/getting-started/tutorial.md)
[http://react-guide.github.io/react-router-cn/docs/guides/basics/RouteConfiguration.html](http://react-guide.github.io/react-router-cn/docs/guides/basics/RouteConfiguration.html)
[（route）https://blog.csdn.net/weixin_51666715/article/details/124077673](https://blog.csdn.net/weixin_51666715/article/details/124077673)
[（route）https://zhuanlan.zhihu.com/p/431389907](https://zhuanlan.zhihu.com/p/431389907)

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

[React Router 中文文档](http://react-guide.github.io/react-router-cn/index.html)

```bash
npm install --save react-router-dom
```

webpack

```bash
npm install webpack -g
```

sass

```bash
npm install sass
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

```html
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

## 5.refs 转发

在下面的示例中，FancyButton 使用 React.forwardRef 来获取传递给它的 ref，然后转发到它渲染的 DOM button：

```typescript
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className='FancyButton'>
    {props.children}
  </button>
));

// 你可以直接获取 DOM button 的 ref：
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

你不能在函数组件上使用 ref 属性，因为他们没有实例。

## 6.Hook 规则

- 只在最顶层使用 Hook
  不要在循环，条件或嵌套函数中调用 Hook， 确保总是在你的 React 函数的最顶层以及任何 return 之前调用他们。
- 只在 React 函数中调用 Hook
  不要在普通的 JavaScript 函数中调用 Hook。你可以：
  - 在 React 的函数组件中调用 Hook
  - 在自定义 Hook 中调用其他 Hook (我们将会在下一页 中学习这个。)

## 7.Fragments | "<>"

React 中的一个常见模式是一个组件返回多个元素。Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。
注：类似 vue 的 template，因为 render 返回只能有一个根元素，有时候用 div 会不支持，比如下面的代码。用这个类似 template 的，就可以解析了

```typescript
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}
```

以下是错误的：

```typescript
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Hello</td>
        <td>World</td>
      </div>
    );
  }
}
```

下面 2 种都是正确的：

```typescript
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
或使用短语法;
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

## 8. state 和 props 之间的区别是什么？

props（“properties” 的缩写）和 state 都是普通的 JavaScript 对象。它们都是用来保存信息的，这些信息可以控制组件的渲染输出，而它们的一个重要的不同点就是：props 是传递给组件的（类似于函数的形参），而 state 是在组件内被组件自己管理的（类似于在一个函数内声明的变量）。

## 9. 为什么 setState 给了我一个错误的值？

调用 setState 其实是异步的 —— 不要指望在调用 setState 之后，this.state 会立即映射为新的值。如果你需要基于当前的 state 来计算出新的值，那你应该传递一个函数，而不是一个对象（详情见下文）。

```typescript
incrementCount() {
  // 注意：这样 *不会* 像预期的那样工作。
  this.setState({count: this.state.count + 1});
}

handleSomething() {
  // 假设 `this.state.count` 从 0 开始。
  this.incrementCount();
  this.incrementCount();
  this.incrementCount();
  // 当 React 重新渲染该组件时，`this.state.count` 会变为 1，而不是你期望的 3。

  // 这是因为上面的 `incrementCount()` 函数是从 `this.state.count` 中读取数据的，
  // 但是 React 不会更新 `this.state.count`，直到该组件被重新渲染。
  // 所以最终 `incrementCount()` 每次读取 `this.state.count` 的值都是 0，并将它设为 1。

  // 问题的修复参见下面的说明。
}
```

给 setState 传递一个函数，而不是一个对象，就可以确保每次的调用都是使用最新版的 state（见下面的说明）。

```typescript
incrementCount() {
  this.setState((state) => {
    // 重要：在更新的时候读取 `state`，而不是 `this.state`。
    return {count: state.count + 1}
  });
}

handleSomething() {
  // 假设 `this.state.count` 从 0 开始。
  this.incrementCount();
  this.incrementCount();
  this.incrementCount();

  // 如果你现在在这里读取 `this.state.count`，它还是会为 0。
  // 但是，当 React 重新渲染该组件时，它会变为 3。
}
```

## 10. Route

- redirect,访问/main 的时候跳转到/event

```typescript
<Route path='/main' element={<Redirect to='/event' />} />
```

- 整页面跳转

```typescript
export default function Login() {
  return (
    <div>
      <h1>Bookkeeper!</h1>
    </div>
  );
}
```

```typescript
function DD() {
  return <Link to='/login'>ddd</Link>;
}
<Routes>
  <Route path='/' element={<DD />} />
  <Route path='/login' element={<Welcome />} />
</Routes>;
```

- 局部跳转

index.tsx

```typescript
const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

app.tsx

```typescript
function App() {
  return (
    <Routes>
      <Route path='/' element={<Login />} />
      <Route path='/main' element={<Main />}>
        <Route path='about' element={<About />} />
        <Route path='fire' element={<Fire />} />
        <Route path='dashboard' element={<DashBoard />} />
      </Route>
    </Routes>
  );
}
```

注意：如果想默认加载 Dashboard,需要这样设置

```typescript
/* <Route path="dashboard" element={<DashBoard />} /> */
<Route index element={<DashBoard />} />
```

main.tsx

```typescript
export default function Main(props: any) {
  return (
    <div className={style.box}>
      <Head />
      <div className={style.content}>
        <div className={style.left}>
          <LeftMenu />
        </div>
        <div className={style.right}>
          <Outlet />
        </div>
      </div>
      <Footer />
    </div>
  );
}
```

注意\<Outlet />标签，main 下面的子路由都会加载到这里。

head.tsx

```typescript
export default function Head(props: any) {
  const navigate = useNavigate();
  return (
    <div className={style.head}>
      <Button onClick={() => navigate('/main/about')}>about</Button>
      <hr />
    </div>
  );
}
```

leftmenu.tsx

```typescriptexport default function LeftMenu(props: any) {
    return (
        <div>
            <ul>
                <li>
                    <Link to="/main/dashboard">dashboard</Link>
                </li>
                <li>
                    <Link to="/main/fire">fire</Link>
                </li>
            </ul>
            <hr />
        </div>
    );
}
```

**可以看到，当使用 button 的时候，要事件跳转，用 Link 则不需要。**

_\<NavLink>与\<Link>组件类似，且可实现导航的“高亮”效果。当当前路由匹配的时候可以设置 active_

```text
useRoutes():作用：根据路由表，动态创建<Routes>和<Route>。
useNavigate():作用：返回一个函数用来实现编程式导航。
useParams():作用：回当前匹配路由的params参数，类似于5.x中的match.params。
useSearchParams():作用：用于读取和修改当前位置的 URL 中的查询字符串。
useLocation():作用：获取当前 location 信息，对标5.x中的路由组件的location属性。
useMatch():作用：返回当前匹配信息，对标5.x中的路由组件的match属性。
```
