# express-router

express路由中间件。中间件自动获取指定目录文件夹中定义的路由，并简化路由配置形式。

## 安装

全局安装或者本地安装:
```
npm install -S express-router
```

## 中间件使用方法

```javascript
const path = require('path');
const express = require('express');
const expressRouter = require('express-router');

const app = express();
// 指定项目目录下routes目录作为路由目录
const routesPath = path.resolve(__dirname, './routes');


// 启用路由中间件
app.use(expressRouter(routesPath));

// ...省略express其它代码

```

## 路由定义格式

```javascript
module.exports = [
    [[sortNum], method, path, callback [, callback ...] ],
    [[sortNum], method, path, callback [, callback ...] ],
    ...
]
```
* `sortNum` Number 可选参数，用来定义路由的声明顺序，值越小，路由声明顺序越靠前
* `method` String 必须参数，用来声明路由的method, 不区分大小写（'all', 'get', 'post',...)，具体参考express method定义
* `path` [String/RegExp/...] 必须参数，路由的路径，支持express所有path配置方式
* `callback` 路由处理方法（可以是中间件），用法与express callback相同



## 路由写法示例

```javascript
// 假定使用了./routes/作为路由目录
// ./routes/test.js中声明路由

/**
 * 路由处理方法1
 */
const testAction1 = (req, res, next) => {
    console.log('test action1');
}

/**
 * 路由处理方法2
 */
const testAction2 = (req, res, next) => {
    res.status(200).status('ok');
}


/**
 * routes配置，配置格式如下：
 * routes = [
 *     ['get', '/abc', fun1, [fun2, fun3, ...]],
 *     ['post', '/abcd', fun1, fun2],
 *     ['GET', '/ab/:params1/:params2', fun1],
 *     ...
 * ]
 */
module.exports = [
    // 音频测试页
    [1, 'get', '/te', testAction2],
    [0, 'get', '/tes', testAction1],
    ['GET', '/test', testAction1, testAction2],
];


```