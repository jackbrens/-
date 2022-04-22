

**一起养成写作习惯！这是我参与「掘金日新计划 · 4 月更文挑战」的第3天，[点击查看活动详情](https://juejin.cn/post/7080800226365145118)**



## 前言

学习 Express 的目的是为了让前端人员也能自己写后端接口，这样在工作时也能站在后端人员的角度上思考问题，对于工作的效率有着看不见的提升~~

本期文章具有知识前提：

- JavaScript 基础知识 （ES6+ 语法）
- Node.js 基础知识
- 最好具备一定的的后端知识（例如Java、Python和PHP等）



## 所用技术

- Node.js  版本号：16.13.0
- express  版本号：4.16.1
- mysql  版本号：2.18.1



## 使用方法

在cmd命令中全局安装[express](https://www.expressjs.com.cn/) 和 `express-generator`：

```bash
# 全局安装express
npm install -g express 
# 全局安装express脚手架
npm install -g express-generator
```



`cmd` 命令查看 `express` 版本号：

```javascript
C:\Users\admin>express --version
4.16.1
```



安装完后，在自己要创建项目的目录下打开cmd，我的目录是 `D:\ProgramFiles`

输入 `express 项目名称` 创建项目，如下：

![image-20220403212404297](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220403212404297.png)



在 `express-demo` 中使用 `npm install` 安装依赖

在浏览器中输入 `localhost：3000`  即可访问

![image-20220403212746950](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220403212746950.png)

为了避免每次修改 js 文件，我们都需要重启服务器，比较麻烦，影响开发效率。所以我们在开发环境中引入 `nodemon` 插件，实现实时热更新，自动重启项目。

我们在开发环境中启动项目应该使用 `npm start` 命令，因为我们在 `package.json` 文件中配置了以下命令：

```
# 安装依赖
npm install nodemon -D

# 在 package.json 中修改
"scripts": {
   "start": "nodemon ./bin/www"
}
```

当我们修改代码时，浏览器也会实时更新。



## MySQL安装和配置

> 使用 npm install mysql 安装 mysql 依赖
>
> 利用 `Navicat` 创建数据库 `express-demo` ，创建 `user` 用户表

![image-20220403214039674](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220403214039674.png)



## 实现 register 接口

根目录新增 `database` 文件夹，新增 `index.js` 文件，代码如下：

```javascript
const mysql = {
  host: 'localhost', // 主机号
  port: '3306', // 端口号
  user: 'root', // 数据库用户名
  password: '123456', // 数据库密码
  database: 'express-demo', // 数据库名称
  connectTimeout: 5000 // 超时时间为5000毫秒
}

module.exports = mysql;
```



根目录新增 `utils` 文件夹，新增 `index.js` 文件，代码如下：

```javascript
const mysql = require('mysql');
const db = require('../database');

// 连接到mysql
function connectMySql () {
  const { host, user, password, database } = db;
  // 使用createConnection方法创建一个表示与mysql数据库服务器之间连接的connection对象
  return mysql.createConnection({
    host,
    user,
    password,
    database
  })
}
```



添加一个查询数据库的函数，返回一个 `new Promise`。返回的结果用 `try catch` 包起来，这样我们就能捕获到如果执行数据库操作时的错误。

```javascript
/**
 * 新建查询语句
 * @param sql
 * @returns {Promise<unknown>}
 */
function querySql (sql) {
  const con = connectMySql();
  return new Promise((resolve, reject) => {
    try {
      con.query(sql, (err, res) => {
        if (err) {
          reject(err);
        } else {
          resolve(res);
        }
      })
    } catch (e) {
      reject(e);
      // finally 的作用是不管访问数据库是否成功，都要关闭连接，否者端口将会被一直占用
    } finally {
      con.end();
    }
  })
}

module.exports = {
  querySql
}
```



根目录新增 `service` 文件夹，新增 `userService.js` 文件，代码如下：

```javascript
const { querySql } = require('../utils');

/**
 * 用户注册
 * @param req 请求参数对象
 * @param res 响应参数对象
 * @param next() 函数  (表示是否继续跳转执行下一个middle或者route函数)
 * @returns {Promise<unknown>}
 */
function register (req, res, next) {
  const { user_name, user_password } = req.body;
  const body = Object.values(req.body);
  for (let item of body) {

    // 如果有一个为空就返回
    if (item === '' || item === null || item === undefined) {
      res.json('用户信息某一项为空');
      return;
    }
  }
  const sql = `insert into user(user_name, user_password)
               values('${user_name}', '${user_password}')`;
  return querySql(sql).then(data => {
    res.json('注册成功');
  }).catch(err => {
    console.log(err);
    res.json('注册失败');
  })

}

module.exports = {
  register
}
```



在目录 `routes/users.js` 中添加一个路由并调用 `register` 方法：

```javascript
var express = require('express');
var router = express.Router();
const service = require('../service/userService');

/* GET users listing. */
router.get('/', function(req, res, next) {
  res.send('respond with a resource');
});

// 添加路由
router.post('/register', service.register);

module.exports = router;
```



在目录 `routes/index.js` 下新增如下代码：

之后在路由首页中导入 `user` 模块并使用，这样的好处就是不需要加一个关于用户逻辑的接口在下面写一段逻辑，而是把他们按照模块区分开来。

```javascript
/* routes/index.js */

var express = require('express');
var router = express.Router();

const userRouter = require('./users'); // 导入user模块

// 使用user中间件，请求地址变为 localhost:300/user/xxx
router.use('/user', userRouter); 

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

module.exports = router;
```



## 接口测试

利用 `postman` 测试接口是否能够成功连接到数据库中。

![image-20220403225104508](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220403225104508.png)

![image-20220403225216528](C:\Users\Jack\AppData\Roaming\Typora\typora-user-images\image-20220403225216528.png)



## 总结

以上就是本次分享的全部内容~~

如果觉得文章写得不错，对你有所启发的，请不要吝啬 点个 `赞` 和 `关注` 并在 `评论区` 留下你宝贵的意见哦~~:smiley:

