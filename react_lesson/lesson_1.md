# 前端工程化 #
前端从根本上来说，就三个基本内容：HTML,JS,CSS。
最早的前端开发，往往就是一个页面就用一个文件搞定，后来把结构（html)、样式(css)、动作(js)分离，用过script，link等引入，这应该就是最早的前端工程化思想。

###前端工程化的表现形式：###

**1.组件化：**就是将页面视为一个容器，页面上各个独立部分例如：头部、侧边菜单栏、底部等视为独立的组件，不同的页面，就是将不同的组件装进一个容器里即可组成完整的浏览器展示页面。

**2.模块化：**所谓的模块化就是封装细节，提供使用接口，彼此之间互不影响，每个模块都是实现某一特定的功能。
ES5以及之前的模块化实现：es5的js本身缺少对文件依赖关系的管理。CommonJS和AMD都是模块化的标准规范，就像ECMAScript是js的规范一样，实现CommonJS规范的api是同步加载模块的，实现AMD规范的api是异步加载模块的（非阻塞加载），更适合浏览器。比如requirejs就是一个js的模块加载器，基于AMD的规范，会异步加载模块。
ES6对于模块化的实现：es6原生支持模块化，通过import导入模块，export导出模块。使用import和export时都必须出现在他们被使用之处的顶层作用域，比如不能放在if条件内部使用，他们必须出现在所有块和函数的外部



 
#一、react#
react是一个用来构建用户界面的js库，主要用来构建UI界面，相当于MVC中的V（视图）
### 特点： ###
1. 高效：通过对DOM的模拟（虚拟DOM），最大限度的减少与DOM的交互,而传统jquery开发方式，都是通过$操作符直接操作Dom树中的真实dom,效率较低
1. 灵活：可以与已知的库或框架很好的配合，例如react-redux,react-router等
1. JSX语法：jsx是js语法的扩展，建议使用jsx
1. 组件：react通过构建组件，最终以组件树的形式呈现完整的浏览器页面，就像搭积木一样，使得代码高复用性，可以很好的运用于大型项目中
1. 单向响应的数据流：
### 链接 ###
[https://reactjs.org/docs/hello-world.html](https://reactjs.org/docs/hello-world.html)



# 二、ES6 #
ES6是一个泛指，含义ECMAScript 5.1(2011年发布）以后的JavaScript的下一代标准，涵盖了ES2015(2015年发布的ECMAScript标准)、ES2016(2016年发布的ECMAScript标准)、ES2017(2017年发布的ECMAScript标准)等等，一般我们说的ES6指的就是ES2015标准，有时也是泛指“下一代Javascript语言”
常用的变化如下：
变量和常量的声明分别用let和const
变量的解构赋值
常用数据类型的扩展
Promise对象
generator函数
箭头函数
### 链接 ###
[http://es6.ruanyifeng.com/](http://es6.ruanyifeng.com/)


# 三、babel babel-polyfill #
是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。比如箭头函数，这个特性还没有得到广泛支持，Babel将其转为普通函数，就能在现有的JavaScript环境执行了。
但是默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。例如，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。



# 四、eslint #
是一个针对JavaScript和JSX的语法规则和代码风格的可插拔的检查工具，要求在nodejs环境中，并在Windows、mac、linux上工作，
用于团队多人开发时，保证写出语法正确、风格统一的代码来，提高项目质量
项目的根目录下.eslintrc文件用于配置ESLint,可以检查项目中的代码是否符合配置文件中预设的规则
rules:{
"no-console":0,//0表示禁用，1表示警告，2表示错误
}
正常开发项目时，使用脚手架搭建空项目，自动将eslint配置进我们的项目里，如果不是用脚手架的话，手动使用eslint，需要安装eslint-->在工程文件夹下运行eslint --init ->然后根据提示一步步回答问题，最后生成.eslintrc.json文件



# 五、ant-design #
蚂蚁金服出品，用react实现的界面ui组件库,主要搭配react，适用于开发企业后台级项目，省去了项目中写大量html结构和css的时间，支持现代浏览器和IE9+(需要polyfills)
### 安装： ###
1、**使用npm安装（推荐）：**

因为可以按需加载，只加载需要用到的组件，下面这两种方式都可以实现按需加载：

**使用babel-plugin-import:**

    // .babelrc or babel-loader option
    {
      "plugins": [
    ["import",
    	 { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }
    	]  // `style: true` 会加载 less 文件
      ]
    }
这样直接在使用时引入模块（如下：），而不需引入样式

    import {DatePicker} from 'antd';   //不需要引入对应的css,因为在webpack配置里有加载less文件

**手动引入：**

    import DatePicker from 'antd/lib/date-picker';   //加载js
    import 'antd/lib/date-picker/style/css';         //加载css

2、**下载已构建的js文件和css文件，然后通过script和link引入（不推荐**）

传统模式，在浏览器中使用 script 和 link 标签直接引入文件，不推荐该用法，是因为没法按需加载，无论是否需要的模块，统统都被加载进来了

### 链接 ###
[https://ant.design/docs/react/introduce-cn](https://ant.design/docs/react/introduce-cn)
