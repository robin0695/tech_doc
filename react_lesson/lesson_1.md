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

```javascript
// .babelrc or babel-loader option
{
    "plugins": [
["import",
        { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }
    ]  // `style: true` 会加载 less 文件
    ]
}
```

这样直接在使用时引入模块（如下：），而不需引入样式

    import {DatePicker} from 'antd';   //不需要引入对应的css,因为在webpack配置里有加载less文件

**手动引入：**

    import DatePicker from 'antd/lib/date-picker';   //加载js
    import 'antd/lib/date-picker/style/css';         //加载css

2、**下载已构建的js文件和css文件，然后通过script和link引入（不推荐**）

传统模式，在浏览器中使用 script 和 link 标签直接引入文件，不推荐该用法，是因为没法按需加载，无论是否需要的模块，统统都被加载进来了

### 链接 ###
[https://ant.design/docs/react/introduce-cn](https://ant.design/docs/react/introduce-cn)

# 六、npm
NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

1. 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
1. 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
1. 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

## 全局安装与本地安装

npm 的包安装分为本地安装（local）、全局安装（global）两种，从敲的命令行来看，差别只是有没有-g而已，比如 

    npm install express          # 本地安装
    npm install express -g       # 全局安装

**本地安装**

    1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
    2. 可以通过 require() 来引入本地安装的包。 

**全局安装**

    1. 将安装包放在 /usr/local 下或者你 node 的安装目录。
    2. 可以直接在命令行里使用。

**如果要查看某个模块的版本号，可以使用命令如下：**

    $ npm list grunt

    projectName@projectVersion /path/to/project/folder
    └── grunt@0.4.1

##NPM 常用命令

NPM提供了很多命令，例如install和publish，使用npm help可查看所有命令。

1. NPM提供了很多命令，例如install和publish，使用npm help可查看所有命令。
1. 使用npm help <command>可查看某条命令的详细帮助，例如npm help install。
1. 在package.json所在目录下使用npm install . -g可先在本地安装当前命令行程序，可用于发布前的本地测试。
1. 使用npm update <package>可以把当前目录下node_modules子目录里边的对应模块更新至最新版本。
1. 使用npm update <package> -g可以把全局安装的对应命令行程序更新至最新版。
1. 使用npm cache clear可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人。
1. 使用npm unpublish <package>@<version>可以撤销发布自己发布过的某个版本代码。

## 使用淘宝 NPM 镜像

大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

    $ npm install -g cnpm --registry=https://registry.npm.taobao.org

这样就可以使用 cnpm 命令来安装模块了：

    $ cnpm install [name]

# 七、webpack
Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。
![avatar](img/what-is-webpack.png)

从图中我们可以看出，Webpack 可以将多种静态资源 js、css、less 转换成一个静态文件，减少了页面的请求。

接下来我们简单为大家介绍 Webpack 的安装与使用。

##安装 Webpack

在安装 Webpack 前，你本地环境需要支持 node.js。

由于 npm 安装速度慢，本教程使用了淘宝的镜像及其命令 cnpm，安装使用介绍参照：使用淘宝 NPM 镜像。

使用 cnpm 安装 webpack：
    
    cnpm install webpack -g

##LOADER

Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。

所以如果我们需要在应用中添加 css 文件，就需要使用到 css-loader 和 style-loader，他们做两件不同的事情，css-loader 会遍历 CSS 文件，然后找到 url() 表达式然后处理他们，style-loader 会把原来的 CSS 代码插入页面中的一个 style 标签中。

    cnpm install css-loader style-loader

安装了 css 或者 style loader后 webpack就可以处理样式文件了。

##配置文件

我们可以将一些编译选项放在配置文件中，以便于统一管理：

webpack.config.js 文件，代码如下所示：

```javascript
module.exports = {
    entry: "./runoob1.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" }
        ]
    }
};
```

##插件

插件在 webpack 的配置信息 plugins 选项中指定，用于完成一些 loader 不能完成的工。

webpack 自带一些插件，你可以通过 cnpm 安装一些插件。

比如我们可以安装内置的 BannerPlugin 插件，用于在文件头部输出一些注释信息。

修改 webpack.config.js，代码如下：

``` javascript
var webpack=require('webpack');

module.exports = {
    entry: "./runoob1.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" }
        ]
    },
    plugins:[
    new webpack.BannerPlugin('菜鸟教程 webpack 实例')
    ]
};

```

##开发环境

当项目逐渐变大，webpack 的编译时间会变长，可以通过参数让编译的输出内容带有进度和颜色。

    webpack --progress --colors

如果不想每次修改模块后都重新编译，那么可以启动监听模式。开启监听模式后，没有变化的模块会在编译后缓存到内存中，而不会每次都被重新编译，所以监听模式的整体速度是很快的。

    webpack --progress --colors --watch

当然，我们可以使用 webpack-dev-server 开发服务，这样我们就能通过 localhost:8080 启动一个 express 静态资源 web 服务器，并且会以监听模式自动运行 webpack，在浏览器打开 http://localhost:8080/ 或 http://localhost:8080/webpack-dev-server/ 可以浏览项目中的页面和编译后的资源输出，并且通过一个 socket.io 服务实时监听它们的变化并自动刷新页面。

    cnpm install webpack-dev-server -g # 安装

    webpack-dev-server --progress --colors # 运行
