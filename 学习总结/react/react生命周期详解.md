# 生命周期

```
/**
 * 关键词说明：
 *      初始化渲染：组件创建第一次调用render方法的时候
 * 
 * 声明周期方法执行顺序：
 *      初始化渲染：
 *          getInitialState (工厂模式方法，现在使用的es6方法，或称typescript方法在constructor里面进行初始化)
 *          componentWillMount
 *          render
 *          componentDidMount
 *      props或state更改(更新)：
 *          componentWillReceiveProps
 *          shouldComponentUpdate 返回true则执行render,false则不执行
 *          componentWillUpdate
 *          render
 *          componentDidUpdate
 *      挂载结束：
 *          componentWillUnmount
 *
 * 常用状态组件API
 *      setState
 *      forceUpdate (常用于PureComponent中，强制刷新)
 *      
 * 状态组件内部属性
 *      defaultProps
 *      displayName
 *      
 *  状态组件实例化属性
 *      props
 *      state
 */

// es5写法，es6写法看新模板，不再重复(其实就是你们天天写的)
// es5写法已被官方唾弃
// 现在推荐采用工厂模式写法(React.createFactory())，纯粹函数式编程，对我们而言难度太高，放弃了
var Child = React.createClass({
    //初始化渲染执行后调用-挂载。仅执行一次
    componentDidMount: function(){
        console.log('Child componentDidMount');
    },
    //组件接收到新的 props 的时候调用。在初始化渲染的时候，该方法不会调用。
    //用此函数可以作为 react 在 prop 传入之后， render() 渲染之前更新 state 的机会。老的 props 可以通过 this.props 获取到。在该函数中调用 this.setState() 将不会引起第二次渲染。
    componentWillReceiveProps: function(nextProps){
        console.log('Child componentWillReceiveProps');
        console.log(nextProps);
    },
    //在接收到新的 props 或者 state，将要渲染之前调用。该方法在初始化渲染的时候不会调用，在使用 forceUpdate 方法的时候也不会
    //此例子中没有state，所以nextState是null
    shouldComponentUpdate: function(nextProps,nextState){
        console.log('Child shouldComponentUpdate');
        console.log(nextProps);
        console.log(nextState);
        return true;
    },
    //在接收到新的 props 或者 state 之前立刻调用。在初始化渲染的时候该方法不会被调用。使用该方法做一些更新之前的准备工作。
    //你不能在刚方法中使用 this.setState()。如果需要更新 state 来响应某个 prop 的改变，请使用 componentWillReceiveProps
    componentWillUpdate: function(nextProps,nextState){
        console.log('Child componentWillUpdate');
        console.log(nextProps);
        console.log(nextState);
    },
    //在组件的更新已经同步到 DOM 中之后立刻被调用。该方法不会在初始化渲染的时候调用。使用该方法可以在组件更新之后操作 DOM 元素。
    //比如首次更新：prevProps: {index: 0}. prevState: null
    componentDidUpdate: function(prevProps,prevState){
        console.log('Child componentDidUpdate');
        console.log(prevProps);
        console.log(prevState);
    },
    //在组件从 DOM 中移除的时候立刻被调用。一般用来销毁组件
    componentWillUnmount: function(){
        console.log('Child componentWillUnmount');
    },
    render: function(){
        console.log('Child render');
        return <div id="child">{this.props.index}</div>;
    }
});

var HelloMessage = React.createClass({
    //初始化this.state
    getInitialState: function(){
        console.log('HelloMessage getInitialState');
        return {
            index: 0
        };
    },
    //初始化渲染执行前调用-挂载。仅执行一次
    componentWillMount: function(){
        console.log('HelloMessage componentWillMount');
    },
    //初始化渲染执行后调用-挂载。仅执行一次
    componentDidMount: function(){
        console.log('HelloMessage componentDidMount');
        var self = this;
        $(this.refs.btnJq).on('click',function(e){
              self.handleChange();
        });
    },
    //在接收到新的 props 或者 state，将要渲染之前调用。该方法在初始化渲染的时候不会调用，在使用 forceUpdate 方法的时候也不会
    //因为props重来没改过，所以nextProps总是{name:'zmr'}
    shouldComponentUpdate: function(nextProps,nextState){
        console.log('HelloMessage shouldComponentUpdate');
        console.log(nextProps);
        console.log(nextState);
        return true;
    },
    //在接收到新的 props 或者 state 之前立刻调用。在初始化渲染的时候该方法不会被调用。使用该方法做一些更新之前的准备工作。
    //你不能在刚方法中使用 this.setState()。如果需要更新 state 来响应某个 prop 的改变，请使用 componentWillReceiveProps
    componentWillUpdate: function(nextProps,nextState){
        console.log('HelloMessage componentWillUpdate');
        console.log(nextProps);
        console.log(nextState);
    },
    //在组件的更新已经同步到 DOM 中之后立刻被调用。该方法不会在初始化渲染的时候调用。使用该方法可以在组件更新之后操作 DOM 元素。
    //比如首次更新：prevProps: {name: 'zmr'}. prevState: {index: 0}
    componentDidUpdate: function(prevProps,prevState){
        console.log('HelloMessage componentDidUpdate');
        console.log(prevProps);
        console.log(prevState);
    },
    //在组件从 DOM 中移除的时候立刻被调用。一般用来销毁组件
    componentWillUnmount: function(){
        console.log('HelloMessage componentWillUnmount');
        $(this.refs.btnJq).off('click');
    },
    handleChange: function(){
        var index = this.state.index;
        index++;
        this.setState({
            index: index
        });
    },
    //渲染
    render: function() {
        console.log('HelloMessage render');
        return (
            <div id="hellowMessage">
                Hello {this.props.name}. index is {this.state.index}
                <a href="javascript:;" onClick={this.handleChange}>点我</a><br/>
                <a href="javascript:;" ref="btnJq">jquery绑定的事件</a><br/>
                <Child index={this.state.index} />
            </div>
        );
    }
});

// 挂载
ReactDOM.render(
  <HelloMessage name="heartblood" />,
  document.getElementById('example')
);

// 移除
// React.unmountComponentAtNode(document.getElementById('example'))  销毁组件。分别调用了HelloMessage.componentWillUnmount和Child.componentWillUnmount
```


