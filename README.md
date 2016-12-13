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

## 启动本地服务器








