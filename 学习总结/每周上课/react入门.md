# react 是什么

Facebook认为MVC无法满足他们的扩展需求，他们期望`以某种方式组织代码，使其更加可预测`

react是view层的js框架

# react和vue

react和vue都是model driven思想的严格践行者，以及通过component拆分完整整个系统的分治

- react社区要比vue大很多，但大都是英文，语言层面不大友好
- react在view层侵入性还是要比vue大很多的
- 渲染性能react略高于vue（仅限于首次渲染，vue1直接使用dom，性能其实更好）
- 对于component的写法，react偏向于all in js，语法学习上需要下一些功夫，而vue配合vue-loader，其实在很大程度上让你不会觉得陌生--这不就是web component么（想到Polymer了么）
- react本身倾向于函数式编程模式

vue本身可认为就是react的改进版，对一些痛点的修补，大大降低学习成本，vue 更符合传统 web 开发的思路，模版样式与逻辑分离

但在我看来，尽管react本身的学习成本过高，但如果平稳度过前期学习阶段，react的开发开发效率和代码健壮性会高于vue，讲真jsx才是符合人的直觉

# 定义component

```js
function test(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

```js
class test extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

```js
const test = <h1>Hello, world</h1>;
```

```js
function Login(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserLogin />;
  }
  return <GuestLogin />;
}
```

# Props are Read-Only

> All React components must act like pure functions with respect to their props.

```js
function sum(a, b) {
  return a + b;
}
```

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```
官方建议不能修改props

```js
 return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
```


```js
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

# state

类似于vue里的data，但是规则较多，区别于props，state属于组建内部变量，不应向外暴露

```js
 class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

```js
this.setState({comment: 'Hello'});
```

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

```
handleInputChange(event) {
    const name = event.target.name;
    this.setState({
      [name]: value
    });
  }
```



# 事件绑定

```js
onClick={activateLasers}
```

```js
// 区分两者
this.activateLasers = this.activateLasers.bind(this);

onClick={activateLasers.bind(this)}
```



event用驼峰写法
禁止默认事件用`preventDefault`


# 双向绑定

vue里存在v-model绑定from表单控件，react里不建议双向绑定写法

表单控制采用单向绑定回调触发修改写法

```js
<input type="text" value={this.state.value} onChange={this.handleChange} />

constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
}
handleChange(event) {
    this.setState({value: event.target.value});
}
```

# react开发思路

1. 分析后端数据，即从Mock入手
2. 分解UI，设计每层React组件，定义层级关系 (即定义props)
3. 建立简易静态页面 
> Sketch, Airbnb 的 React Sketch.app

4. 思考页内UI State状态
5. 思考state的生命周期
6. 添加数据流
7. 照着之前的思路实现


# 技术向写法

> if 

```js
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}
```

```js
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick.bind(this)} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

```

> if-else 

```js
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}

render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

> 阻止render

```js
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

```

> 渲染多个component

```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

# 扩展

create-react-app

