# 依赖技术点

- webpack
- babel 语法解析
- es6/7 基本语法
- eslint 语法检查
- npm scripts 统一的任务构建
- react 组件化 基础类库
- mocha
- karma

# 学习步骤

## 基本命令

> 初始化

```
    $ npm install webpack --save-dev
    $ touch webpack.config.js
    $ mkdir src
    $ cd src
    $ touch index.js component.js
```

> webpack.config.js

```
   var path = require('path');
   var webpack = require('webpack')
   
   var config = {
       entry:path.resolve(__dirname,'./src/index.js'),
       output:{
           path:path.resolve(__dirname,'dist'),
           filename:'bundle.js'
       }
   }
   
   module.exports = config;
```

> component.js

```
   module.exports = function(){
       document.body.innerHTML = "搭建开发环境"
   }
```

> index.js

```
    var component = require('./component')
    
    component()
```

> index.html

```
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <script src="./dist/bundle.js"></script>
    </body>
    </html>
```

> package.json

```
      "scripts": {
        "build":"webpack --progress --colors"
      },
```

> 运行

```
    $ npm run build
```

## 使用es6

> 初始化

```
    $ touch .babelrc
    $ npm install babel-loader babel-core babel-preset-es2015 --save-dev
```

> .babelrc

```
    {
      "presets":["es2015"]
    }
```

> webpack.config.js

```
    +
     module:{
            loaders:[
                {
                    test:/\.js$/,
                    loader:'babel',
                    exclude:/node_modules/
                }
            ]
        }
```

> component.js

```
   export default function(){
       document.body.innerHTML = "搭建开发环境"
   }
```

> index.js

```
    import component from './component'
    
    component()
```

> 运行

```
    $ npm run build
```


## 自动产出html

> 初始化 

```
    $ mv index.html src
    $ npm install html-webpack-plugin --save-dev
```


> webpack.config.js

```
    +
    var htmlWebpackPlugin = require('html-webpack-plugin')
    
    +
     plugins:[
        new htmlWebpackPlugin({
            title:"搭建前端工作流",
            template:'./src/index.html'
        })
     ]
```

> index.html

```
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title><%= htmlWebpackPlugin.options.title %></title>
    </head>
    <body>
    </body>
    </html>
```

> 运行

```
    $ npm run build
```

## 启动本地服务器

> 初始化

```
    npm install webpack-dev-server --save-dev
```

> 配置

package.json

```
    "scripts": {
         "dev":"webpack-dev-server --progress --port 8080 --content-base dist --hot"
      }
```

或 package.json

```
    "scripts": {
       "dev":"webpack-dev-server --progress"
     }
```

webpack.config.js

```
    +
     devServer:{
            contentBase:'dist',
            inline:true,
            port:8080,
            stats:{
                color:true
            }
        },
```

> 运行

```
    $ npm run dev
```

## 打包react

> 初始化

```
    $ npm install react react-dom --save-dev
    $ npm install babel-preset-react --save-dev
    $ cd src
    $ mkdir components containers
    $ rm -rf components.js
    $ cd components
    $ touch Header.js Footer.js
    $ cd ../containers
    $ touch App.js
```

> .babelrc

```
    {
      "presets":["es2015","react"]
    }
```

> Header.js

```
    import React ,{ Component} from 'react'
    
    export default class Header extends Component{
        render(){
            return <div>
                <h1> 我是header </h1>
            </div>
        }
    }
```

> Footer.js

```
    import React ,{ Component} from 'react'
    
    class Footer extends Component{
        render(){
            return <div>
                <h1> 我是footer </h1>
            </div>
        }
    }
    
    export default  Footer
```

> App.js

```
    import React , { Component } from 'react'
    import Header from '../components/Header'
    import Footer from '../components/Footer'
    
    export default class App extends Component{
        render() {
            return (
                <div>
                    <Header />
                    <Footer />
                </div>
            )
        }
    }
```

> index.js

```
    import  React from 'react'
    import  { render } from 'react-dom'
    import App from './containers/App'
    
    render(<App />,document.getElementById('app'))
```

> index.html

```
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title><%= htmlWebpackPlugin.options.title %></title>
    </head>
    <body>
        <div id="app"></div>
    </body>
    </html>
```

> 运行

```
    $ npm run dev
```

## 在webpack中加载css

> 初始化

```
    $ npm install style-loader css-loader --save-dev
    $ npm install less less-loader --save-dev
    $ cd src/components
    $ mkdir Header Footer
    $ mv Header.js Header
    $ mv Footer.js Footer
    $ cd ../containers
    $ mkdir App
    $ mv App.js App
    $ cd App
    $ mv App.js index.js
    $ touch index.css
    $ cd ../../components/Footer
    $ mv Footer.js index.js
    $ touch index.css
    $ touch index.less
    $ cd ../Header
    $ mv Header.js index.js
    $ touch index.css
```

> Footer/index.js   

```
    import React ,{ Component} from 'react'
    import './index.css'
    import './index.less'
    
    
    class Footer extends Component{
        render(){
            return <div>
                <h1 className="footer"> 我是footer </h1>
                <h1 className="footer2"> 我是footer2 </h1>
    
            </div>
        }
    }
    
    export default  Footer
```

> Footer/index.css

```
    .footer{
        color: red;
    }
```

> Footer/index.less

```
    @color: #00ff00;
    
    .footer2 {
      background-color: @color;
    }
```

> webpack.config.js

```
    module:{
            loaders:[
                {
                    test:/\.js$/,
                    loader:'babel',
                    exclude:/node_modules/
                },
                {
                    test:/\.css$/,
                    loader:'style!css',
                    include:path.resolve(__dirname,'src')
                },
                {
                    test:/\.less/,
                    loader:'style!css!less',
                    include:path.resolve(__dirname,'src')
                }
            ]
        },
```

> 运行

```
    npm run dev
```