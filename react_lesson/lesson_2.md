# 一、react基础知识
## 1.创建react组件
我们知道react开发就是用JavaScript去构造UI组件的过程，但是组件到底长什么样的，该如何开发？简单的说，一个react组件就是一个js文件，内含render方法，返回组件UI的描述。
**注意： 
（1） render方法的return后面只能返回一个根节点
（2）{}中的内容被当成js表达式解析**

常用创建组件的方式有 **3** 种（组件类的首字母必须大写）：
**（1）**.  ES6方式定义（extends React.Component）：现在推荐的创建有状态组件的方式，如下所示：
```javascript
class HelloWorld extends React.Component {
    render () {
        return (
            <h1>Hello World!</h1>
        )
    }
}
```
有时候我们会看到别人的代码中，并不是extends React.Component这样写的，而是extends Component，那是因为在该js文件的最上方写了import { Component } from 'react'，所以可以写成class HelloWorld extends Component

**（2）**.  函数式定义：这种方式是用来创建无状态组件，该组件只需要根据其父组件传来的props进行渲染UI界面，而不需要涉及到state状态的操作,在大部分react代码中，大多数组件被写出无状态组件，通过简单的组合可以构建成其他的组件等；这种通过多个简单然后合并成一个大应用的设计模式被提倡。函数式定义组件的方式如下：
```javascript
function HelloWorld (props){
    return (
        <h1>Hello World!</h1>
    );
}
```
**（3）**.  ES5原生方式定义（React.createClass）：这是react刚开始时推荐的创建组件的方式，现在已经不推荐了，形式如下：
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
**2.1 props**
组件的嵌套就存在父子组件的关系，`props就是react中的一种从父组件向子组件传输数据的机制。`因为props是从父组件传入子组件，所以在子组件中不能被修改。那么props的具体作用是什么呢，请看下面代码：
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

上述代码中，我们在组件中通过this.props获取组件的属性，this.props对象的属性与组件的属性一一对应，但有个例外`this.props.children`,他表示组件的所有子节点：
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
**2.2 state**
前面我们介绍的是props的用法，它是组件所接收的属性对象，并且不可以去改变它，正因为不可改变它，导致了组件是静态的，只能默默的接收值然后渲染界面。如果此时想要给组件添加行为和交互，组件就需要有可变化的数据来体现自己的状态（state）。react组件可以在`this.state`里面保存可变化的数据，并且`this.state`是该组件私有的，可以通过调用`this.setState()`函数修改state。
我们可以在类的构造函数里给组件设置一个初始state,如下所示：
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
前面我们给div绑定的click事件是这样写的：
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
**4.1 JSX**
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
因为JSX是JavaScript，所以我们不能使用JavaScript保留字，这包括class和for，所以设置标签上class类时使用`className`,还有一些其他属性，如标签上的属性`for`转换为`htmlFor`。
综上，我们发现JSX不是必须的，我们在开发过程中也可以使用React.createElement方式来呈现标签结构，但是在多个标签嵌套的情况下，使用起来很麻烦：如下，所以我们推荐使用JSX。
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
**4.2 React数据绑定**

