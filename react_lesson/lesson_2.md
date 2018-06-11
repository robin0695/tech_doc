# 一、react基础知识
## 1.创建react组件
我们知道react开发就是用JavaScript去构造UI组件的过程，但是组件到底长什么样的，该如何开发？简单的说，一个react组件就是一个js文件，内含render方法，返回组件UI的描述。

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