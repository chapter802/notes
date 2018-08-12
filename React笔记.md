#React 笔记

##### 警告:
* 组件名称必须以大写字母开头。
* 组件的返回值只能有一个根元素。


#### 零碎知识点
* 构造函数是唯一能够初始化 this.state 的地方。
* React 可以将多个setState() 调用合并成一个调用来提高性能。
* redux的核心概念: 将需要修改的state都存入到store里，发起一个action用来描述发生了什么，用reducers描述action如何改变state tree 。创建store的时候需要传入reducer，真正能改变store中数据的是store.dispatch API。
* 父子关系子级向父级通信，父级可以将一个回调函数当做属性传递为子级，子级可以直接调用函数从而和父级通信。

``` JavaScript
    this.setState((prevState, props) => ({
    counter: prevState.counter+ props.increment
}));
``` 

* 标签特性采取驼峰式大小写风格
* 所有元素必须闭合
* 三元表达式

```javascript

render() {
    return (
        <div className={condition ? "salutation" : ""}>
            Hello JSX!
        </div>
    )
}
// 有条件的渲染整个节点
<div>
    {condition ? <span>Hello JSX!</span> : null}
</div>
//条件外置
render() {
    let  className;
    if (condition) {
        className = "salutation";
    }
    return (
        <div className={className}>Hello JSX</div>
    )
}
```

* 显式插入空格{" "}, React不会在多行的元素之间渲染一个空格
* 注释:处于标签的子区域中时, 用{}来包围注释
* 把Markdown转换为HTML  https://github.com/chjj/marked  npm install —save marked

```javascript
//元素工厂   解构赋值
import React, { Component } from 'react';
import {render} from 'react-dom';

let {
    form,
    input
}=React.DOM;

class CommentForm extends Component {
    render() {
        return form({className:"commentForm"},
            input({type:"text",placeholder:"name"}),
            input({type:'text',placeholder:"comment"}),
            input({type:"submit",value:"Post"})
            )
    }
}
```

* 自定义propTypes 校验器

```javascript
import React, {component, propTypes } from 'react';

let titlepropType = (props, propName, componentName) => {
    
    if (props[propName]) {
        let value = props[propName];
        if (typeof value !== 'string' || value.length > 80) {
            return new Error(
                `${propName} in ${componentName} is longer than 80 characters.`
            );
        }
    }
};
```
* React不变性助手 update  // npm install -sacve react-addons-update

```javascript
let new student = update(student, {grades:{$set: ['A', 'B', 'C']}});
```

* Keys可以在DOM中的某些元素被增加或删除的时候帮助React识别哪些元素发生了变化。因此你应当给数组中的每一个元素赋予一个确定的标识。元素的key只有在它和它的兄弟节点对比时才有意义。
* Super(props) 子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。
<hr />
* 不要将合成事件与原生事件混用 通过 e.target 判断来避免。，用 reactEvent.nativeEvent. stopPropagation() 来阻止冒泡是不行的。
* redux-actions 并不是 middleware, 只是用来生成基本 action type 函数模版代码而已,是一个 action creator 吧，同时他的 handleactions 可以简化 reducers 的写法 不用那么多 switch ，总的来说只是一个代码生成的辅助，不用也是可以的。
* React 合成事件阻止冒泡 e.preventDefault() 

* 哪些组件应该有 State？
    1. 大部分组件的工作应该是从 props 里取数据并渲染出来。但是，有时需要对用户输入、服务 器请求或者时间变化等作出响应，这时才需要使用 State。
    2. 尝试把尽可能多的组件无状态化。 这样做能隔离 state，把它放到最合理的地方，也能减少冗 余并，同时易于解释程序运作过程。
    3. 常用的模式是创建多个只负责渲染数据的无状态（stateless）组件，在它们的上层创建一个 有状态（stateful）组件并把它的状态通过 props 传给子级。这个有状态的组件封装了所有用 户的交互逻辑，而这些无状态组件则负责声明式地渲染数据。

* 哪些应该作为 State？
    1. State 应该包括那些可能被组件的事件处理器改变并触发用户界面更新的数据。
    
    
* 哪些不应该作为 State？ this.state 应该仅包括能表示用户界面状态所需的最少数据。 不应该包括：
    1. 计算所得数据： 把所有的计算都放到 render() 里更容易保证用户界面和数据的一致性。
    2. React 组件：在 render() 里使用当前 props 和 state 来创建它。
    3, 基于 props 的重复数据： 尽可能使用 props 来作为实际状态的源。把 props 保存到 state 的一个有效的场景是需要知道它以前值的时候，因为 props 可能因为父组件重绘的 结果而变化。

* 当 React 校正带有 key 的子级时，它会确保它们被重新排序（而不是破坏）或者删除（而不 是重用）。 务必把 key 添加到子级数组里组件本身上，而不是每个子级内部最外层 HTML 上。
* 注意为了性能考虑，只在开发环境验证 propTypes 。
* 对于复合组件，引用会指向一个组件类的实例所以你可以调用那个类定义的任何方法。
* React有props和state: props意味着父级分发下来的属性，state意味着组件内部可以自行管理的状态，并且整个React没有数据向上回溯的能力，也就是说数据只能单向向下分发，或者自行内部消化。
* 数据绑定 mapStateToProps   事件绑定 mapDispatchToProps

##### Reactjs 小书

* 当某个状态被多个组件依赖或者影响的时候，就把该状态提升到这些组件的最近公共父组件中去管理，用 props 传递数据或者函数来管理这种依赖或着影响的行为。
* 一般会把组件的 state 的初始化工作放在 constructor 里面去做；在 componentWillMount 进行组件的启动工作，例如 Ajax 数据拉取、定时器的启动；组件从页面上销毁的时候，有时候需要一些数据的清理，例如定时器的清理，就会放在 componentWillUnmount 里面去做。
* `dangerouslySetInnerHTML={{__html: this.state.content}}` 动态设置元素的 innerHTML 会导致跨站脚本攻击（XSS）, 不建议使用。

* 组件的内容编写顺序如下：
  
  1. static 开头的类属性，如 defaultProps、propTypes。
  2. 构造函数，constructor。
  3. getter/setter。
  4. 组件生命周期。
  5. _ 开头的私有方法。
  6. 事件监听方法，handle*。
  7. render 开头的方法，有时候 render() 方法里面的内容会分开到不同函数里面进行，这些函数都以 render  开头。
  8. render() 方法。
  
  
* 高阶组件 高阶组件就是一个函数，传给它一个组件，它返回一个新的组件。新的组件使用传入的组件作为子组件。作用是用于代码复用，可以把组件之间可复用的代码、逻辑抽离到高阶组件当中。新的组件和传入的组件通过 props 传递信息。

* 纯函数 函数的返回结果只依赖于它的参数；函数执行过程里面没有副作用。
* 所有的 Dumb 组件都放在 components/ 目录下，所有的 Smart 的组件都放在 containers/ 目录下，这是一种约定俗成的规则。
* Dumb 根据 props 进行渲染。而 Smart 则是负责应用的逻辑、数据，把所有相关的 Dumb（Smart）组件组合起来，通过 props 控制它们。
* Smart 组件可以使用 Smart、Dumb 组件；而 Dumb 组件最好只使用 Dumb 组件，否则它的复用性就会丧失。
* 要根据应用场景不同划分组件，如果一个组件并不需要太强的复用性，直接让它成为 Smart 即可；否则就让它成为 Dumb 组件。而 Dumb 则是可以跨应用场景复用，Smart 和 Dumb 都可以复用，只是程度、场景不一样。
* 高阶组件 警告
  1. 不要在render函数中使用高阶组件
  2. 静态方法必须复制
  3. Refs不会被传递
  
* 要获取一个 React 组件的引用，既可以使用 this 来获取当前 React 组件，也可以使用 refs 来获取你拥有的子组件的引用。
* 无状态组件挂载时只是方法调用，没有新建实例。对于 React 组件来说，refs 会指向一个组件类的实例，所以可以调用该类定义的任何方法。




```javascript
    componentDidMount() {
  
        document.body.addEventListener('click', e => { 
            if (e.target && e.target.matches('div.code')) {
                return;
            }
        
        this.setState({ 
            active: false, 
            }); 
        });
    }

    coffee.filter(i => i !== value);
    
    //相当于
    coffee.filter(function (i) {
      return i !== value;
    });
    
    // constructor 和 getInitialState 的区别
    // ES6
    class MyComponent extends React.Component {
      constructor(props) {
        super(props);
        this.state = { /* initial state */ };
      }
    }
    
    // 相当于 ES5
    var MyComponent = React.createClass({
      getInitialState() {
        return { /* initial state */ };
      },
    });
    
    
    
```

##### 路由

* browserHistory
* hashHistory
* createMemoryHistory

* 如果需要在 Home 路由被渲染后才激活的指向 / 的链接，请使用 `<IndexLink to="/">Home</IndexLink>`.
* Polyfill的准确意思为：用于实现浏览器并不支持的原生API的代码。
* react-router-dom 比 react-router 多了个<Link> <BrowserRouter> 这样的 DOM 类组件。
* 在 Route component 中，以 this.props.location 获取
* 在 Route render 中，以 ({location}) => () 方式获取
* 在 Route children 中，以 ({location}) => () 方式获取
* 在 withRouter 中，以 this.props.location 的方式获取


#### Redux

* 三大原则:
    1. 单一数据源    // store
    2. State 是只读的   // action
    3. 使用纯函数来执行修改   // reducers
* Reducers 指定了应用状态的变化如何响应 actions 并发送到 store 的，actions 只是描述了有事情发生了这一事实，并没有描述应用如何更新 state。
* reducer 是不允许有副作用的。不能在里面操作 DOM，也不能发 Ajax 请求，更不能直接修改 state，它要做的仅仅是 —— 初始化和计算新的 state。
* Store 就是把action, reducers联系到一起的对象。Redux 应用只有一个单一的 store。 Store 有以下职责：
    1. 维持应用的 state；
    2. 提供 getState() 方法获取 state;
    3. 提供 dispatch(action) 方法更新 state；
    4. 通过 subscribe(listener) 注册监听器;
    5. 通过 subscribe(listener) 返回的函数注销监听器。
* 当需要拆分数据处理逻辑时，使用 reducer 组合而不是创建多个 store。
* 展示组件 容器组件
* Thunk 函数的含义  编译器的"传名调用"实现，往往是将参数放到一个临时函数之中，再将这个临时函数传入函数体。这个临时函数就叫做 Thunk 函数。

##### 数据流 
严格的单向数据流是 Redux 架构的设计核心。
* Redux 应用中数据的生命周期遵循下面 4 个步骤：
    1. 调用 store.dispatch(action)。
    2. Redux store 调用传入的 reducer 函数。
    3. 根 reducer 应该把多个子 reducer 输出合并成一个单一的 state 树。
    4. Redux store 保存了根 reducer 返回的完整 state 树。



#### 全家桶系列

##### axios

##### redux
* applyMiddleware是Redux的一个原生方法，将所有中间件组成一个数组，依次执行。 中间件多了可以当做参数依次传进去。


##### react-redux
* react-redux提供了connect和Provider两个方法，它们一个将组件与redux关联起来，一个将store传给组件。组件通过dispatch发出action，store根据action的type属性调用对应的reducer并传入state和这个action，reducer对state进行处理并返回一个新的state放入store，connect监听到store发生变化，调用setState更新组件，此时组件的props也就跟着变化。
* 如果只使用redux，那么流程是这样的： `component --> dispatch(action) --> reducer --> subscribe --> getState --> component`
  用了react-redux之后流程是这样的： `component --> actionCreator(data) --> reducer --> component`
* Provider是一个组件，它接受store作为props，然后通过context往下传，这样react中任何组件都可以通过context获取store。也就意味着我们可以在任何一个组件里利用dispatch(action)来触发reducer改变state，并用subscribe监听state的变化，然后用getState获取变化后的值。但是并不推荐这样做，它会让数据流变的混乱，过度的耦合也会影响组件的复用，维护起来也更麻烦。
* connect -- `connect(mapStateToProps, mapDispatchToProps, mergeProps, options)` 是一个函数，它接受四个参数并且再返回一个函数--wrapWithConnect，wrapWithConnect接受一个组件作为参数wrapWithConnect(component)，它内部定义一个新组件Connect(容器组件)并将传入的组件(ui组件)作为Connect的子组件然后return出去。
  所以它的完整写法是这样的：`connect(mapStateToProps, mapDispatchToProps, mergeProps, options)(component)`
*  react --> redux --> react 流程：
        1. Provider组件接受redux的store作为props，然后通过context往下传。 
        2. connect函数收到Provider传出的store，然后接受三个参数mapStateToProps，mapDispatchToProps和组件，并将state和actionCreator以props传入组件，这时组件就可以调用actionCreator函数来触发reducer函数返回新的state，connect监听到state变化调用setState更新组件并将新的state传入组件。

##### [redux-actions](https://github.com/reduxactions/redux-actions)
* createAction\(s\)
    1. createAction(type, payloadCreator, metaCreator)
    2. createActions(actionMap, ...identityActions)
* handleAction\(s\)
    1. handleAction(type, reducer, defaultState)
    2. handleAction(type, reducerMap, defaultState)
    3. handleActions(reducerMap, defaultState)
* combineActions
    1. combineActions(...types)
   


##### [react-dom](https://github.com/facebook/react/tree/master/packages/react-dom)
* react-dom
    1. findDOMNode
    2. render
    3. unmountComponentAtNode
* react-dom/server
    1. renderToString
    2. renderToStaticMarkup

##### [react-router](https://github.com/ReactTraining/react-router)

* Histories
    1. browserHistory       // 使用浏览器中的 History API 用于处理 URL
    2. hashHistory
    3. createMemoryHistory


##### [react-responsive](https://github.com/contra/react-responsive)

##### [redux-thunk](https://github.com/reduxjs/redux-thunk)
* redux-thunk 是redux 异步 action 中间件。
* redux-thunk最重要的思想，就是可以接受一个返回函数的action creator。如果这个action creator 返回的是一个函数，就执行它，如果不是，就按照原来的next(action)执行。


#### React API




#### 相关知识点

* [normalizr](https://github.com/paularmstrong/normalizr)   // JSON数据范式化
* [Immutable](https://facebook.github.io/immutable-js/)     // 数据创建后不可改变
* [Reselect](https://github.com/reactjs/reselect)           
* [Ant Design](https://ant.design/docs/react/introduce-cn)  // Ant Design 的 React 实现，开发和服务于企业级后台产品
* [jest](https://github.com/facebook/jest)                  // facebook 的 JavaScript 测试 
* [react-leaflet](https://github.com/PaulLeCam/react-leaflet)   // 国外react 地图插件
* [marked](https://github.com/chjj/marked)                  // 把Markdown转换为HTML
* [i18next](https://github.com/i18next/react-i18next)       // 中英文切换 国际化
* [redux-form](https://github.com/erikras/redux-form)       // 表单提交
* [react-motion](https://github.com/chenglou/react-motion)  // react动画



#### MobX

* MobX 会对在执行跟踪函数期间读取的任何现有的可观察属性做出反应。
* 黄金法则: 如果你想创建一个基于当前状态的值时，请使用 computed。
* 如果你有一个函数应该自动运行，但不会产生一个新的值，请使用autorun。





#### React 编程思想

##### 第一步：把UI拆分为一个组件的层级

* 单一功能原则（single responsibility principle），也就是一个组件在理想情况下只做一件事情。如果它最终增长了，它就应该被分解为更小的组件。
* DRY 原则：Don't Repeat Yourself

##### 第二步：用React创建一个静态版本

##### 第三步：确定最小（但完备）的 UI state 表达

* 检查 props 和 state

    1. 它是通过props从父级传递来的吗？如果是，它可能不是 state。

    2. 它随时间变化吗？如果不是,它可能不是 state。

    3. 你能基于其他任何组件里的 state 或者 props 计算出它吗？如果是,它可能不是state.

##### 第四步：确定你的 state 应该存在于哪里

* React 总是在组件层级中单向数据流动的。

    1. 确定哪些组件要基于 state 来渲染内容。 
    2. 找到一个共同的拥有者组件（在所有需要这个state组件的层次之上，找出共有的单一组 件）。 
    3. 要么是共同拥有者，要么是其他在层级里更高级的组件应该拥有这个state。 
    4. 如果你不能找到一个组件让其可以有意义地拥有这个 state，可以简单地创建一个新的组 件 hold 住这个state，并把它添加到比共同拥有者组件更高的层级上。

##### 第五步：添加反向数据流






#### 新概念

* 跨切面关注点


#### 其它（招聘App笔记）
* MD5 第三方库 [utility]()



```javascript
// md5密码加盐加密
function md5Pwd(pwd) {
  const salt = 'imooc_is_good_145756xadf8gafd?@#$%UBJKH32'
  return utils.md5(utils.md5(pwd + salt))
}

```
