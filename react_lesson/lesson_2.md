# 一、react基础知识

## 1.创建react组件
我们知道react开发就是用JavaScript去构造UI组件的过程，但是组件到底长什么样的，该如何开发？简单的说，一个react组件就是一个js文件，内含render方法，返回组件UI的描述。
**注意： 
（1） render方法的return后面只能返回一个根节点
（2）{}中的内容被当成js表达式解析**

常用创建组件的方式有 **3** 种（组件类的首字母必须大写）：
**（1）**.  ES6方式定义（`extends React.Component`）：现在推荐的创建有状态组件的方式，如下所示：
```javascript
class HelloWorld extends React.Component {
    render () {
        return (
            <h1>Hello World!</h1>
        )
    }
}
```
有时候我们会看到别人的代码中，并不是`extends React.Component`这样写的，而是`extends Component`，那是因为在该js文件的最上方写了`import { Component } from 'react'`，所以可以写成`class HelloWorld extends Component`

**（2）**.  函数式定义：这种方式是用来创建无状态组件，该组件只需要根据其父组件传来的`props`进行渲染UI界面，而不需要涉及到`state`状态的操作,在大部分react代码中，大多数组件被写出无状态组件，通过简单的组合可以构建成其他的组件等；这种通过多个简单然后合并成一个大应用的设计模式被提倡。函数式定义组件的方式如下：
```javascript
function HelloWorld (props){
    return (
        <h1>Hello World!</h1>
    );
}
```
**（3）**.  ES5原生方式定义（`React.createClass`）：这是react刚开始时推荐的创建组件的方式，现在已经不推荐了，形式如下：
```javascript
var HelloWorld = React.createClass({
    render:function (){
        return (
            <h1>Hello World!</h1>
        );
    }
});
```
## 2.将组件拼装组合起来
react开发时尽量创建简单并且可复用的组件，然后将各个组件进行嵌套和组合来构建复杂的UI界面，上面已经介绍了一个react组件的基本结构以及创建，接下来介绍如何将组件嵌套组合起来。
###2.1 props
组件的嵌套就存在父子组件的关系，`props就是react中的一种从父组件向子组件传输数据的机制。`因为`props`是从父组件传入子组件，所以在子组件中不能被修改。那么`props`的具体作用是什么呢，请看下面代码：
```javascript
import react,{Component} from 'react'
//父组件
class ParentComponent extends Component {
    render () {
        //父组件中嵌套子组件，并且通过添加属性的方式传值name和age
        return (
            <ul>
                <ChildComponent name="zhangsan" age="18"/>
                <ChildComponent name="lisi" age="20"/>
            </ul>
        )
    }
}
//子组件
class ChildComponent extends Component {
    render () {
        //{}中的内容被当成js表达式解析
        //子组件中接收父组件传过来的值，name和age都在this.props中
        return (
            <li>{this.props.name}:{this.props.age}岁</li>
        )
    }
}
//将父组件渲染到id='root'的节点中
React.render(<ParentComponent/>,document.getElementById("root"));
```

上述代码中，我们在组件中通过`this.props`获取组件的属性，`this.props`对象的属性与组件的属性一一对应，但有个例外`this.props.children`,他表示组件的所有子节点：
```javascript
import react,{Component} from 'react'
//父组件
class ParentComponent extends Component {
    render () {
        //父组件中嵌套子组件，并且通过添加属性的方式传值name和age
        return (
            <ul>
                <ChildComponent name="zhangsan">18</ChildComponent>
                <ChildComponent name="lisi">20</ChildComponent>
            </ul>
        )
    }
}
//子组件
class ChildComponent extends Component {
    render () {
        //{}中的内容被当成js表达式解析
        //子组件中接收父组件传过来的值:
        //1.this.props.name是使用该子组件时传的值“zhangsan lisi”等，
        //2.this.props.children是使用该子组件时前置标签<ChildComponent>和后置标签</ChildComponent>之间的内容“18  20”等
        return (
            <li>{this.props.name}:{this.props.children}岁</li>
        )
    }
}
//将父组件渲染到id='root'的节点中
React.render(<ParentComponent/>,document.getElementById("root"));
```
###2.2 state
前面我们介绍的是`props`的用法，它是组件所接收的属性对象，并且不可以去改变它，正因为不可改变它，导致了组件是静态的，只能默默的接收值然后渲染界面。如果此时想要给组件添加行为和交互，组件就需要有可变化的数据来体现自己的状态（`state`）。react组件可以在`this.state`里面保存可变化的数据，并且`this.state`是该组件私有的，可以通过调用`this.setState()`函数修改state。
我们可以在类的构造函数里给组件设置一个初始`state`,如下所示：
```javascript
class HelloWorld extends Component {
    constructor () {
        super(...arguments);
        this.state = {
            showDetails:false,//是否显示详情，默认不显示
        }
    }
    render () {
        let details;
        //初始state中的showDetails为true时，才渲染details内容
        if(this.state.showDetails){
            details = (
                <div>我是details内容</div>
            );
        }
        //初始页面中只有“标题”，没有“详情”，给标题绑定click事件来切换详情的显示与隐藏，写法是箭头函数（ES6语法）
        //在react中，属性一律采用驼峰式命名（onClick,而不是onclick）
        return (
            <div>
                <div onClick={ () => {this.setState({showDetails:!this.state.showDetails})} }>
                    我是标题
                </div>
                { details }
            </div>
        );
    }
}
```

## 3.React中的事件
我们知道js原生事件在不同浏览器之间存在差异，这就导致我们在编写代码时需要考虑各个浏览器的兼容性，而平时使用的jquery都帮我们做了这些事，所以我们用react开发时，react实现了一个合成事件系统，它通过标准化来实现一致性，使得事件对象在不同的浏览器和平台间都能拥有相同的属性和方法
前面我们给`div`绑定的`click`事件是这样写的：
`onClick={ () => {this.setState({showDetails:!this.state.showDetails})} }`

还可以在render之外定义好方法：
```javascript
toggleDetails () {
    this.setState({showDetails:!this.state.showDetails});
}
onClick={ this.toggleDetails.bind(this) }
```

或者我们在构造函数里给绑定好this:
```javascript
constructor(props){
    this.toggleDetails = this.toggleDetails.bind(this);
}
<div onClick={ this.toggleDetails }></div>
```

为什么需要bind(this)??如果使用箭头函数是否还需要bind(this)??


## 4.JSX与React中的数据绑定
###4.1 JSX
所有的react组件都有一个render函数，它指定了react组件的HTML输出，JSX即JavaScript eXtension,它是一个react扩展，允许我们编写看起来像html的JavaScript代码。虽然将js和标签代码包含在同一个地方是一个很不好的习惯，但是结果是将视图与功能相结合使得对视图的推理变得非常简单。
假如我们有一个react组件来呈现一个`h1`标签，JSX允许我们以非常类似HTML的方式声明这个元素：
```javascript
class Header extends React.Component {
    render () {
        return (
            <h1>Hello World!</h1>
        );
    }
}
```
虽然看起来很像html，但它本质上是用`React.createElement()`这种方式来编写声明的，JSX在运行时被翻译成常规的JavaScript，上述这个组件翻译后，看起来像这样：
```javascript
class HelloWorld extends React.Component {
    render(){
        return (
            React.createElement('h1',{className:'large'},'Hello World!');
        );
    }
}
```
因为JSX是JavaScript，所以我们不能使用JavaScript保留字，这包括`class`和`for`，所以设置标签上class类时使用`className`,还有一些其他属性，如标签上的属性`for`转换为`htmlFor`。
综上，我们发现JSX不是必须的，我们在开发过程中也可以使用`React.createElement`方式来呈现标签结构，但是在多个标签嵌套的情况下，使用起来很麻烦：如下，所以我们推荐使用JSX。
```javascript
<div>
    <img src="1.png" alt="logo">
    <h1>welcome back!</h1>
</div>
//传送到浏览器的JavaScript看起来像这样：
React.createElement("div",null,
    React.createElement("img",{src:"1.png",alt:"logo"}),
    React.createElement("h1",null,"welcome back!")
);
```
###4.2 React数据绑定
react项目是由多个组件拼装成的，所以每个独立的组件显示一部分独立的ui界面，界面上的数据绑定就是在组件内部完成的。组件的render方法里`return`一段ui描述，我们可以通过父组件传过来的数据`this.props`，返回一段已经绑定好数据的jsx代码:
```javascript
class MyTitle extends React.Component {
    getDefaultProps:function(){
        return {
            title:"hello world"
        }
    },
    propTypes:{
        title:React.PropTypes.string.isRequired,
    },
    render:function(){
        renturn <h1>{this.props.title}</h1>
    }
}
```
上述代码中，我们定义了一个名为`MyTitle`的组件，主要渲染呈现一个`h1`标签，内容是父组件传过来的`title`,其中：

 1. 组件类的 `getDefaultProps`方法可以用来设置组件属性的默认值，如果父组件没传值过来，就使用默认值，如果父组件传值了，就覆盖默认值。
 2. 组件类的`propTypes`属性，就是用来验证组件实例的属性是否符合要求，title必填且为字符串
 3.  render函数里`return`的`h1`标签里的内容就是我们需要绑定的数据，通过{}里面写父组件传过来的`title`,前面我们说过JSX代码里的{}中的内容被当成js表达式解析，常见的有`三目运算符、map`操作等:

```javascript
class MyTitle extends React.Component {
    //也可以在类构造函数外部写：
    //MyTitle.defaultProps = {title:"jack"}
    //MyTitle.propTypes={title:React.PropTypes.string.isRequired,}
    getDefaultProps:function(){
        return {
            title:"jack"
        }
    },
    propTypes : {
        title:React.PropTypes.string.isRequired,
    },
    render : function(){
        //如果父组件传过来的是jack，渲染的就是<div><span>the title is jack</span></div>
        //否则，渲染的就是<div><span>the title is rose</span></div>
        return (
            <div>
            {
                this.props.title == "jack" ?
                <span>the title is jack</span> :
                <span>the title is rose</span>
            }
            </div>
        )
    },
}
```


## 5.React组件的生命周期
学习React，了解它的生命周期是很重要的。组件生命周期一般指的是：组件从被创建到它消亡的过程，因此，组件生命周期方法就是指：在这个过程（创建 ->消亡）中，在一个特定的点会被调用的方法。如果你只是单纯地想用一个无状态的JavaScript函数（没有state）来定义一个React组件,那么这个组件没有生命周期。
###5.1 装载过程
组件的装载过程就是初始创建阶段：
`1.constructor -> 2.componentWillMount -> 3.render -> 4.componentDidMount`
**1. constructor**
`constructor`是ES6的写法，我们用ES6的语法`class`来定义一个类，类里面含有构造函数`constructor`和其他方法，构造函数里的`this`指向类的实例对象，具体参照 [ES6-class](http://es6.ruanyifeng.com/#docs/class#Class%E7%9A%84%E7%BB%A7%E6%89%BF)
constructor方法里我们一般用来设置state的初始值或者绑定事件：
```javascript
//constructor中使用this的话，必须在该方法的第一行写上super()
//只有一个理由需要传递props作为super()的参数，那就是你需要在构造函数内使用this.props,所以我们建议直接写成super(props)
constructor(props) {  
  super(props);  
  this.state = {searchStr: ''};  
  this.handleChange = this.handleChange.bind(this);  
} 
```

**2. componentWillMount**
这个函数被调用一次，在初始渲染之前被调用，注意：这个函数里面设置state不会重新触发渲染

**3. render**

**4. componentDidMount**
在初始化渲染执行之后立刻调用一次,在生命周期中的这个时间点，组件拥有一个 DOM 展现，你可以获取相应 DOM 节点。如果想和其它 JavaScript 框架集成，使用 `setTimeout` 或者 `setInterval` 来设置定时器，或者发送 AJAX 请求，可以在该方法中执行这些操作。

###5.2 更新过程 
state或props的更改，触发组件的更新过程：
`1.componentWillReceiveProps -> 2.shouldComponentUpdate -> 3.componentWillUpdate -> 4.render -> 5.componentDidUpdate`
**1. componentWillReceiveProps**
在组件接收到新的 `props` 的时候调用(只要父组件的render方法被调用，在render函数里被渲染的子组件就会经历更新过程，该组件就会接收到新的`props`,所以新的`props`可能跟上次的`props`一样)。在初始化渲染的时候，该方法不会调用。
**注意：**
1.  官网上说：`在该函数中调用 this.setState() 将不会引起第二次渲染。`
子组件显示父组件传过来的`props`有2中方式，一种是直接将`props`里的数据绑定到标签上显示,所以我们不需要做什么就发生一次渲染然后呈现最新的界面。另一种是在`componentWillReceiveProps`里将传过来的`props`转为自己的`state`，**引起第二次渲染** ：是指每次子组件接收到新的`props`，都会重新渲染一次,但是你可以在这次渲染前，根据新的`props`更新`state`(就是`componentWillReceiveProps`方法里内容)，更新`state`也会触发一次重新渲染，但react不会这么傻，所以只会渲染一次，这对应用的性能是有利的。
2.  只有`props`的"变化"会导致这个函数的调用，`state`的变化不会调用这个函数：在该组件里this.setState触发的更新过程不会调用`componentWillReceiveProps`，因为`componentWillReceiveProps`这个方法内部就是根据`nextProps`判断是否需要更新state，如果更新state触发的更新过程又调用`componentWillReceiveProps`，那么就成死循环了。

**2. shouldComponentUpdate**
无论`props`还是`state`的变化导致的组件更新过程都会调用这个函数。
这是一个特殊的函数，它会在render函数之前被调用，在这个函数里可以定义是否需要一次渲染，或者这次渲染可以跳过，这个函数对性能优化而言很有用。


**3. componentWillUpdate**
无论`props`还是`state`的变化导致的组件更新过程都会调用这个函数
当接收到新的`props`或`state`而进行渲染之前，此函数被调用，这个函数中不允许通过`this.setState`对`state`进行更改，他只能严格的用于准备即将开始的渲染，而不能自己再去触发一次更新。
**4. render**

**5. componentDidUpdate**
无论`props`还是`state`的变化导致的组件更新过程都会调用这个函数。
在组件的渲染执行之后立刻调用。

###5.3 卸载过程 
`componentWillUnmount`
在一个组件被卸载时立即调用，可以进行一些清理操作。

