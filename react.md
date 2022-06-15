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

- any:任意赋值
- unknown：严谨，必须先判断类型
  let da:unknown=1
  da='fff';
  if(typeof da === 'string')
  {
  da.toString();
  }
- never,抛异常用
  function gets():never
  {
  throw new Error('')
  }

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

举一个例子

```typescript
const handleReset = () => {
  console.log(groupId, groupName);
  setGroupId('');
  setGroupName('');
  console.log(groupId, groupName);
  filterData(groupId, groupName);
};
```

最后结果是

```log
handleSearch
TableHeader.tsx:25 aa
TableHeader.tsx:28 aa
index.tsx:65 handleSearch
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

## 11. React.memo() 是什么？

组件仅在它的 props 发生改变的时候进行重新渲染。通常来说，在组件树中 React 组件，只要有变化就会走一遍渲染流程。但是通过 PureComponent 和 React.memo()，我们可以仅仅让某些组件进行渲染。
[https://www.cnblogs.com/zhangguicheng/articles/12897605.html](https://www.cnblogs.com/zhangguicheng/articles/12897605.html)

## 12. React 中样式的 3 种方式

### 12.1 style

<details>
<summary>style文件</summary>

```typescript
//首先是样式文件 style.ts
const useStyles = {
  paper: {
    width: '400px',
    border: '1px solid #E7EBF0',
    position: 'absolute',
    top: '50%',
    left: '50%',
    transform: 'translate(-50%,-50%)',
    textAlign: 'center',
    verticalAlign: 'center',
    padding: '40px 48px',
    borderRadius: '5px',
  } as React.CSSProperties,
  title: {
    fontWeight: '800',
  },
  divider: {
    marginBottom: '10px',
  },
  grid: { marginTop: '10px' },
  label: {
    textAlign: 'left',
  } as React.CSSProperties,

  textfield: {},
  button: { marginTop: '25px', borderRadius: '5px' },
};
export default useStyles;
```

</details>

在页面文件中

<details>
<summary>页面文件</summary>

```typescript
import { Button, Divider, TextField, Typography } from '@material-ui/core';
import Grid from '@material-ui/core/Grid';
import Paper from '@material-ui/core/Paper';
import { useNavigate } from 'react-router-dom';
import useStyles from './style';

export default function Login() {
  const navigate = useNavigate();

  return (
    <>
      <Paper variant='outlined' style={useStyles.paper}>
        <Typography
          variant='caption'
          display='block'
          style={useStyles.title}
          gutterBottom
        >
          Login Page
        </Typography>
        <Divider style={useStyles.divider} />
        <Grid container direction='column' justifyContent='center'>
          <Grid item style={useStyles.grid}>
            <Typography display='block' gutterBottom style={useStyles.label}>
              email:
            </Typography>
            <TextField
              variant='outlined'
              id='username'
              placeholder='user name'
              size='small'
              fullWidth
              style={useStyles.textfield}
            />
          </Grid>
          <Grid item style={useStyles.grid}>
            <Typography display='block' gutterBottom style={useStyles.label}>
              password:
            </Typography>
            <TextField
              type='password'
              variant='outlined'
              id='userpwd'
              placeholder='user pwd'
              size='small'
              fullWidth
              style={useStyles.textfield}
            />
          </Grid>
          <Button
            variant='contained'
            color='primary'
            fullWidth
            style={useStyles.button}
            onClick={() => navigate('/home')}
          >
            login
          </Button>
        </Grid>
      </Paper>
    </>
  );
}
```

</details>

### 12.2 通过 class 来控制

<details>
<summary>makeStyles 文件</summary>

```typescript
//style.ts文件
import { red } from '@material-ui/core/colors';
import { makeStyles } from '@material-ui/core/styles';

const useStyles = makeStyles((theme) => ({
  topGrid: {
    height: '110px',
    backgroundColor: '#fff',
  },
  input: { width: '173px', height: '30px' },
  inputTxt: { paddingRight: '20px' },
  searchBox: {
    display: 'flex',
    alignItems: 'center',
    paddingLeft: '20px',
  },
  buttonGroup: {
    display: 'flex',
    alignItems: 'center',
    paddingRight: '20px',
  },
  serachButton: {
    backgroundColor: '#744DFE',
    marginRight: '10px',
    color: '#fff',
  },
  resetButton: {
    color: '#656D92',
    marginRight: '20px',
  },
  addButton: {
    color: '#1AD188',
  },
  deleteButton: {
    color: '#DF1847',
  },
  tableHeader: {
    backgroundColor: '#F4F4F6',
    height: '30px',
    padding: '-16px',
  },
  tableBody: {
    // backgroundColor: '#F4F4F6',
    height: '30px',
    padding: '-16px',
  },
  checkColumn: {
    paddingBottom: '0px',
    paddingTop: '0px',
    width: '100px',
  },
  link: { textDecoration: 'none', color: '#744DFE' },
  formrow: {
    padding: '10px',
    marginLeft: '30px',
    display: 'flex',
    alignItems: 'center',
  },
}));
export default useStyles;
```

</details>

在 tsx 文件

<details>
<summary>在 tsx 文件</summary>

```typescript
import {
  TableContainer,
  Table,
  TableHead,
  TableRow,
  TableCell,
  TableBody,
  Button,
  Checkbox,
  Grid,
  Typography,
  InputBase,
  TextField,
  Divider,
  Box,
  TableFooter,
  TablePagination,
  Select,
  MenuItem,
  Modal,
  Fade,
  Backdrop,
  Dialog,
  DialogTitle,
  DialogContentText,
  DialogActions,
  DialogContent,
} from '@material-ui/core';
import { Link as RouteLink } from 'react-router-dom';
import Link from '@material-ui/core/Link';
import AddCircleOutlineIcon from '@material-ui/icons/AddCircleOutline';
import RemoveCircleOutlineIcon from '@material-ui/icons/RemoveCircleOutline';
import MuiButton from '../component/MuiButton';
import useStyles from './style';
import PageSelect from '../../components/PageSelect';
import { useState } from 'react';
import platoformDataType, {
  bodyData,
  platformColumns,
} from '../../mock/platformData';

export default function Platform() {
  const classes = useStyles();

  const [open, setOpen] = useState<boolean>(false);

  const initialData = {
    id: '',
    groupId: '',
    groupName: '',
    note: '',
  };
  const [selectdRow, setSelectdRow] = useState<platoformDataType>(initialData);

  const handleOpen = (id: string) => {
    setOpen(true);
    console.log('add');
    setSelectdRow(bodyData.find((item) => item.id === id) ?? initialData);
  };

  const handleClose = () => {
    setOpen(false);
  };
  return (
    <>
      <TableContainer component='div'>
        <Grid
          container
          direction='row'
          justifyContent='space-between'
          alignItems='center'
          className={classes.topGrid}
        >
          <Box className={classes.searchBox}>
            设置组 ID:
            <TextField
              variant='outlined'
              size='small'
              className={classes.inputTxt}
            />
            设置组名称:
            <TextField variant='outlined' size='small' />
          </Box>
          <Box className={classes.buttonGroup}>
            <Button variant='contained' className={classes.serachButton}>
              查询
            </Button>
            <Button variant='outlined' className={classes.resetButton}>
              重置
            </Button>
            <MuiButton
              text='增加'
              endIcon={<AddCircleOutlineIcon />}
              style={classes.addButton}
              onClick={() => handleOpen('')}
            />
            <MuiButton
              text='删除'
              endIcon={<RemoveCircleOutlineIcon />}
              style={classes.deleteButton}
              onClick={() => {}}
            />
          </Box>
        </Grid>
        <Divider />

        <Table>
          <TableHead>
            <TableRow className={classes.tableHeader} component='tr'>
              <TableCell className={classes.checkColumn}>
                <Checkbox />
              </TableCell>
              {platformColumns.map((item, index) => {
                return <TableCell key={index}>{item}</TableCell>;
              })}
              <TableCell style={{ width: '100px' }}>操作</TableCell>
            </TableRow>
          </TableHead>
          <TableBody>
            {bodyData.map((item) => {
              return (
                <TableRow
                  className={classes.tableBody}
                  component='tr'
                  key={item.id}
                >
                  <TableCell className={classes.checkColumn}>
                    <Checkbox />
                  </TableCell>
                  <TableCell>
                    <Link
                      href='#'
                      className={classes.link}
                      onClick={(e) => handleOpen(item.id)}
                    >
                      {item.groupId}
                    </Link>
                  </TableCell>
                  <TableCell>{item.groupName}</TableCell>
                  <TableCell>{item.note}</TableCell>
                  <TableCell>
                    <Link
                      href='#'
                      className={classes.link}
                      onClick={(e) => handleOpen(item.id)}
                    >
                      明细
                    </Link>
                  </TableCell>
                </TableRow>
              );
            })}
          </TableBody>
        </Table>

        <Grid
          container
          direction='row'
          justifyContent='flex-end'
          alignItems='center'
        >
          共10页
          <PageSelect />
        </Grid>
      </TableContainer>
      {/* 当 fullWidth 属性为true时，对话框将根据 maxWidth 的值进行自我调整。 */}
      <Dialog
        fullWidth={true}
        maxWidth={'sm'}
        open={open}
        onClose={handleClose}
        aria-labelledby='form-dialog-title'
      >
        {/* <DialogTitle id='form-dialog-title'>{selectdRow.groupName}</DialogTitle> */}
        <DialogContent>
          <DialogContentText></DialogContentText>
          <form autoComplete='off'>
            <div className={classes.formrow}>
              <Typography style={{ width: '120px' }}>设置组ID：</Typography>
              <TextField
                required
                autoFocus
                variant='outlined'
                margin='dense'
                id='id'
                value={selectdRow.groupId}
                style={{ width: '173px' }}
              />
            </div>
            <div className={classes.formrow}>
              <Typography style={{ width: '120px' }}>设置组名称：</Typography>
              <TextField
                margin='dense'
                id='name'
                style={{ width: '173px' }}
                variant='outlined'
                value={selectdRow.groupName}
              />
            </div>
            <div className={classes.formrow}>
              <Typography
                style={{
                  width: '120px',
                }}
              >
                说明：
              </Typography>
              <TextField
                multiline
                maxRows={4}
                required
                margin='dense'
                variant='outlined'
                id='note'
                value={selectdRow.note}
                style={{ width: '382px' }}
              />
            </div>
          </form>
        </DialogContent>
        <DialogActions>
          <Button onClick={handleClose} color='primary' variant='contained'>
            提交
          </Button>
          <Button onClick={handleClose} variant='outlined'>
            返回
          </Button>
        </DialogActions>
      </Dialog>
    </>
  );
}
```

</details>

### 12.3 通过 css 文件来控制

<details>
<summary>css/summary>

```typescript
//style.module.css
.paper {
  width: 400px;
  border: 1px solid #e7ebf0;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  vertical-align: center;
  padding: 40px 48px;
  border-radius: 5px;
}
.title {
  font-weight: 800 !important;
  line-height: 24px;
  font-size: 16px !important;
}
.divider {
  /* margin-bottom: 10px !important; */
  margin-top: 10px !important;
}
.grid {
  margin-top: 10px !important;
}
.label {
  text-align: left;
}

.button {
  margin-top: 25px !important;
  border-radius: 5px;
}
```

</details>

页面文件

<details>
<summary>页面文件</summary>

```typescript
import { Button, Divider, TextField, Typography } from '@material-ui/core';
import Grid from '@material-ui/core/Grid';
import Paper from '@material-ui/core/Paper';
import { useNavigate } from 'react-router-dom';
import styles from './style.module.css';

export default function Login() {
  const navigate = useNavigate();

  return (
    <>
      <Paper variant='outlined' className={styles.paper}>
        <Typography
          variant='caption'
          display='block'
          className={styles.title}
          gutterBottom
        >
          Login Page
        </Typography>
        <Divider className={styles.divider} />
        <Grid container direction='column' justifyContent='center'>
          <Grid item className={styles.grid}>
            <Typography display='block' gutterBottom className={styles.label}>
              email:
            </Typography>
            <TextField
              variant='outlined'
              id='username'
              placeholder='user name'
              size='small'
              fullWidth
              className={styles.textfield}
            />
          </Grid>
          <Grid item className={styles.grid}>
            <Typography display='block' gutterBottom className={styles.label}>
              password:
            </Typography>
            <TextField
              type='password'
              variant='outlined'
              id='userpwd'
              placeholder='user pwd'
              size='small'
              fullWidth
              className={styles.textfield}
            />
          </Grid>
          <Button
            variant='contained'
            color='primary'
            fullWidth
            className={styles.button}
            onClick={() => navigate('/home')}
          >
            login
          </Button>
        </Grid>
      </Paper>
    </>
  );
}
```

</details>

## 13. 关于往 dialog 传 detail 数据

传过去的 detailData，如果是只做 view，那么可以在代码中

```typescript
<TextField
 required
 autoFocus
 variant='outlined'
 margin='dense'
 id='groupId'
 value={detailData.groupId}
 style={{ width: '173px' }}
```

但是如果是要兼容做编辑，就不行了，无法改变 textbox 的值，应该是 detailData 是非响应式的
需要

```typescript
const [user, setUser] = useState<platoformDataType>({ ...detailData });
```

```typescript
<TextField
  required
  autoFocus
  variant='outlined'
  margin='dense'
  id='groupId'
  value={user.groupId}
  style={{ width: '173px' }}
  onChange={(e) => valueChange('groupId', e)}
/>
```

但是又牵扯一个问题
user.groupId 的值居然是空的。
<code>const [user, setUser] = useState<platoformDataType>({ ...detailData });
</code>
这行代码应该是只有第一次打开的时候才执行的，后面 dialog 打开不会重新渲染。所以必须再加一行

```typescript
useEffect(() => {
  setUser({ ...detailData });
}, [detailData]);
```

## 14. foreach map

foreach 不返回新的数组，如果是 useState 则不会引起 dom 刷新

```typescript
//table 表格展示的数据
const [bodyData, setBodyData] =
  useState<Array<platoformDataType>>(platoformData);
```

```typescript
bodyData.foreach((item) => {
  if (item.id === id) {
    item = { ...saveData };
  }
});
setBodyData(newData);
```

上面代码不会引起 dom 的更新。要用下面的

```typescript
const newData = bodyData.map((item) =>
  item.id === saveData.id ? { ...saveData } : item
);
setBodyData(newData);
```

或者用

```typescript
const newData = bodyData.filter((item) => item.checked === false);
setBodyData(newData);
```

## 15. react 进阶之 userProduce,Context

Context:避免组件直接用 props 传值，可以免除多重父子组件传值难问题
userProduce：把事件集中起来处理。

第一步，新建一个 producer，这里主要处理各种方法

这里注意的是“state: Array<platoformDataType>”这个参数的值，实际就是上一个调用 reducer 后返回给页面的 state 的值。

```typescript
import platoformDataType from '../mock/platformData';

const platformReducer = (state: Array<platoformDataType>, action: any) => {
  console.log(state);
  switch (action.type) {
    case 'search':
      return action.state.filter(
        (item: platoformDataType) =>
          item.groupId.indexOf(action.groupId) > -1 &&
          item.groupName.indexOf(action.groupName) > -1
      );
    case 'delete':
      return action.state.filter((item: platoformDataType) => !item.checked);
    case 'add':
      return [action.state, ...state];
    case 'edit':
      // return action.state;
      return state.map((item: platoformDataType) =>
        item.id === action.state.id ? { ...action.state } : item
      );
    case 'check':
      return action.state.map((item: platoformDataType) => {
        return item.id === action.id
          ? { ...item, checked: !item.checked }
          : item;
      });
    case 'checkAll':
      return action.state.map((item: platoformDataType) => {
        return { ...item, checked: action.status };
      });
    case 'showAll':
      return action.state;
    default:
      return state;
  }
};
export default platformReducer;
```

第二步页面

```typescript
import { TableContainer, Divider } from '@material-ui/core';
import { createContext, useEffect, useReducer } from 'react';
import platoformDataType, { platoformData } from '../../mock/platformData';
import platformReducer from '../../store/platform';
import PlatFormTableBody1 from './components/TableBody';
import PlatFormTableHeader1 from './components/TableHeader';

//一个默认初始值，好几个地方用到，所以抛出了。
export const initialData = {
  id: '',
  groupId: '',
  groupName: '',
  note: '',
  checked: false,
};
//PlatformContext.Provider  定义这个value的值类型
export const PlatformContext = createContext({
  data: [] as Array<platoformDataType>,
  dispatch: (action: any) => {},
});

export default function Platform1() {
  //调用我们上面创建的platformReducer，这个和useState类似，一个是值，一个是处理值的方法。state就是dispatch执行完后返回的方法
  const [state, dispatch] = useReducer(platformReducer, platoformData);

  return (
    <PlatformContext.Provider value={{ data: state, dispatch }}>
      <>
        <TableContainer component='div'>
          <PlatFormTableHeader1 />
          <Divider />
          <PlatFormTableBody1 />
        </TableContainer>
      </>
    </PlatformContext.Provider>
  );
}
```

第三步让我们看看 PlatFormTableHeader1 页面

<details>
<summary>PlatFormTableHeader1</summary>

```typescript
import { Button, Grid, TextField, Box } from '@material-ui/core';
import AddCircleOutlineIcon from '@material-ui/icons/AddCircleOutline';
import RemoveCircleOutlineIcon from '@material-ui/icons/RemoveCircleOutline';
import MuiButton from '../../component/MuiButton';
import styles from '../style.module.css';
import { useContext, useState } from 'react';
import { PlatformContext, initialData } from '..';
import PlatformDetail1 from '../../platform1/components/Detail';
import platoformDataType, { platoformData } from '../../../mock/platformData';

const PlatFormTableHeader1 = () => {
  //获取data以及改变data的方法
  const { data, dispatch } = useContext(PlatformContext);
  //当前点击新增按钮的时候，默认是initialData
  const [platform, setPlatform] = useState(initialData);
  //查询条件1
  const [groupId, setGroupId] = useState<string>('');
  //查询条件2
  const [groupName, setGroupName] = useState<string>('');
  //dialog是否显示
  const [open, setOpen] = useState<boolean>(false);
  //查询
  const handleSearch = () => {
    dispatch({
      type: 'search',
      state: data,
      groupId,
      groupName,
    });
  };
  //重置
  const handleReset = () => {
    // console.log(platoformData);
    dispatch({ type: 'showAll', state: platoformData });
    setGroupId('');
    setGroupName('');
  };
  //点击新增
  const handleAdd = () => {
    setOpen(true);
  };
  //点击删除
  const handleDelete = () => {
    dispatch({
      type: 'delete',
      state: data,
    });
  };
  //关闭dialog，数据重置
  const handleClose = () => {
    setOpen(false);
    //重新初始化
    setPlatform({ ...initialData });
  };
  //新增保存
  const handleSave = (saveData: platoformDataType) => {
    // console.log(saveData);
    dispatch({
      type: 'add',
      state: saveData,
    });
    setOpen(false);
    //重新初始化
    setPlatform({ ...initialData });
  };
  return (
    <>
      <Grid
        container
        direction='row'
        justifyContent='space-between'
        alignItems='center'
        className={styles.topGrid}
      >
        <Box className={styles.searchBox}>
          设置组ID:
          <TextField
            variant='outlined'
            size='small'
            value={groupId}
            className={styles.inputTxt}
            onChange={(e) => setGroupId(e.target.value)}
          />
          设置组名称:
          <TextField
            variant='outlined'
            size='small'
            value={groupName}
            className={styles.inputTxt}
            onChange={(e) => setGroupName(e.target.value)}
          />
        </Box>
        <Box className={styles.buttonGroup}>
          <Button
            variant='contained'
            className={styles.serachButton}
            onClick={handleSearch}
          >
            查询
          </Button>
          <Button
            variant='outlined'
            className={styles.resetButton}
            onClick={handleReset}
          >
            重置
          </Button>
          <MuiButton
            text='增加'
            endIcon={<AddCircleOutlineIcon />}
            style={styles.addButton}
            onClick={handleAdd}
          />
          <MuiButton
            text='删除'
            endIcon={<RemoveCircleOutlineIcon />}
            style={styles.deleteButton}
            onClick={handleDelete}
          />
        </Box>
      </Grid>
      <PlatformDetail1
        detailData={platform}
        open={open}
        handleClose={handleClose}
        handleSave={handleSave}
      />
    </>
  );
};
export default PlatFormTableHeader1;
```

</details>

<details>
<summary>PlatFormTableBody1</summary>

```typescript
import {
  Table,
  TableHead,
  TableRow,
  TableCell,
  TableBody,
  Checkbox,
  Grid,
} from '@material-ui/core';
import Link from '@material-ui/core/Link';

import styles from '../style.module.css';
import PageSelect from '../../../components/PageSelect';
import platoformDataType, { platformColumns } from '../../../mock/platformData';
import { useContext, useEffect, useState } from 'react';
import { initialData, PlatformContext } from '..';
import PlatformDetail1 from '../../platform1/components/Detail';

export default function PlatFormTableBody1() {
  //当前点击新增按钮的时候，默认是initialData
  const [platform, setPlatform] = useState(initialData);
  const { data, dispatch } = useContext(PlatformContext);
  const [checkedAll, setCheckedAll] = useState<boolean>(false);
  //dialog是否显示
  const [open, setOpen] = useState<boolean>(false);
  //某一行的选择
  const handleCheck = (id: string) => {
    dispatch({ type: 'check', state: data, id });
  };
  //表头的全选
  const handleCheckAll = () => {
    dispatch({ type: 'checkAll', state: data, status: !checkedAll });
    setCheckedAll(!checkedAll);
  };
  //编辑
  const handleEdit = (item: platoformDataType) => {
    setOpen(true);
    setPlatform(item);
  };
  //关闭dialog，数据重置
  const handleClose = () => {
    setOpen(false);
    //重新初始化
    setPlatform({ ...initialData });
  };
  //编辑后保存
  const handleSave = (saveData: platoformDataType) => {
    dispatch({
      type: 'edit',
      state: saveData,
    });
    setOpen(false);
    //重新初始化
    setPlatform({ ...initialData });
  };
  //页面更新后，根据数据的check状态，判断是否全选的checkbox被选中
  useEffect(() => {
    setCheckedAll(data?.length > 0 && data.every((item) => item.checked));
  }, [data]);

  return (
    <>
      <Table className={styles.mainGrid}>
        <TableHead>
          <TableRow className={styles.tableHeader} component='tr'>
            <TableCell className={styles.checkColumn}>
              <Checkbox onClick={handleCheckAll} checked={checkedAll} />
            </TableCell>
            {platformColumns.map((item, index) => {
              return <TableCell key={index}>{item}</TableCell>;
            })}
            <TableCell style={{ width: '100px' }}>操作</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {data.map((item) => {
            return (
              <TableRow
                className={styles.tableBody}
                component='tr'
                key={item.id}
              >
                <TableCell className={styles.checkColumn}>
                  <Checkbox
                    onChange={() => handleCheck(item.id)}
                    checked={item.checked}
                  />
                </TableCell>
                <TableCell>
                  <Link
                    href='#'
                    className={styles.link}
                    onClick={(e) => handleEdit(item)}
                  >
                    {item.groupId}
                  </Link>
                </TableCell>
                <TableCell>{item.groupName}</TableCell>
                <TableCell>{item.note}</TableCell>
                <TableCell>
                  <Link
                    href='#'
                    className={styles.link}
                    onClick={(e) => handleEdit(item)}
                  >
                    明细
                  </Link>
                </TableCell>
              </TableRow>
            );
          })}
        </TableBody>
      </Table>
      <Grid
        container
        direction='row'
        justifyContent='flex-end'
        alignItems='center'
      >
        共10页
        <PageSelect />
      </Grid>
      <PlatformDetail1
        detailData={platform}
        open={open}
        handleClose={handleClose}
        handleSave={handleSave}
      />
    </>
  );
}
```

</details>
