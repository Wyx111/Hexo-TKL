---
title: 使用NPM，Webpack，Babel配置React开发环境
---

Facebook于2013年开源了用于架设Instagram网站用的库--React，彻底前端开发UI组件的思路。在使用React的时候，我们可以使用Reac脚手架来生成开发环境。这里我们选择了使用NPM，Webpack，Babel从零来搭建一个简易的React开发环境。

### 安装，配置Webpack

​		本质上，*webpack* 是一个现代 JavaScript 应用程序的*静态模块打包器(module bundler)*。

我们一般选择在全局安装webpack ，就可以在终端进行webpack指令。

```
npm install  webpack -g
```

首先，初始化一个项目。

```
cd Desktop/
mkdir webpack
cd webpack/
npm init 这里也可以使用npm init -y
```

这个时候文件夹中会生成一个package.json

开始安装webpack

```
npm install --save webpack
```

webpack的是通过一个webpack.config.js来工作的。

```
touch webpack.config.js
vim webpack.config.js
```

开始编辑webpack.config.js

```
var webpack = require('webpack')
var path = require('path')
//设置入口路径
var APP = path.resolve(__dirname, 'src/client/app')
//设置出口路径
var BUNDLE = path.resolve(__dirname, 'src/client/public')
var config = {
    entry: APP + '/index.js',
    output: {
        path: BUNDLE,
        fileName: "bundle.js"
    }
}
module.exports = config;
```

接下来,在目录中 ./src/client/app中创建index.js 

```
console.log('hello, world!')
```

打开终端

```
webpack -d 
```

这时候在./src/client 自动创建了public文件夹，bundle.js就是webpack编译过后文件。

在./src/client/创建 index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
	<div id="app"></div>
</body>
<script src='./public/bundle.js'></script>
</html>
```

打开浏览器，就可以在调试台看到 打印出来的 hello,world!

### 设置Babel-loader

React推荐使用JSX和ES6语法，但是JSX，ES6很多浏览器的支持性并不高，Babel可以帮助我们对语法进行转换。

```
npm i babel-loader babel-preset-es2015 babel-preset-react -S
```

创建.babelrc

```
touch .babelrc
vim .babelrc
```

```
{
    "presets": ["es2015", "react"]
}
```

接下来在之前配置好的webpack.config.js中添加

```
var config = {
	...
    module: {
        rules: [{
            test: /\.js$/,
            include: APP_DIR,
            use: [{
                loader: 'babel-loader'
            }]
        }]
    }
};
```

完成后，开始写React。

### Hello，world！

```
npm install react-dom
```

修改./src/client/app  index.js 为 index.jsx

```
import React from 'react';
import { render} from 'react-dom';
class App extends React.Component {
	render(){
		return (
				<div>hello World！</div>
			)
	}
}
render(<App />,document.getElementById('app'))
```

打开终端

```
webpack -d
```

打开浏览器就可以在页面中看到hello World！

如果你觉得每一次修改文件都要在终端进行编译。

```
webpack -d --watch
```

这样，每一次的修改保存后，webpack都会进行编译。

### 使用NPM作为执行工具

打开package.json 进行修改。

```
{
 "scripts": {
    "dev": "webpack -d --watch"
  }
}
```

现在我们在终端中执行

```
npm run dev 
```

效果和之前是一样的。

这就是一个简易的React开发环境，后续将对这个配置文件进行扩充，并利用websocket完成一个网页聊天室。
##### &nbsp;
##### &nbsp;
##### &nbsp;

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　2018-09-07  晴

　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　河南郑州