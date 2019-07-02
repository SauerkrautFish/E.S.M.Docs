## node.js前端开发
### 16340270 杨捷
目录结构
- db 数据库存储目录
- models 数据库模型文件目录
- public 公共文件目录（css,js,img）
- routers 路由文件目录
- schemas 数据库结构文件
- views 模板视图文件目录
- app.js 启动文件
- package.json
- 1app.js 文件

1.创建应用、监听端口

```
const app = express();
 
app.get('/',(req,res,next) => {
  res.send("Hello World !");
});
app.listen(3000,(req,res,next) => {
  console.log("app is running at port 3000");
});
```

2.配置应用模板
-  定义使用的模板引擎 app.engine('html',swig.renderFile) 参数1：模板引擎的名称，同时也是模板文件的后缀 参数2：表示用于解析处理模板内容的方法
- 设置模板文件存放的目录 app.set('views','./views')
- 注册所使用的模板引擎 app.set('view engine','html')

3.用模板引擎去解析文件

```
  /**
  *读取views目录下的指定文件，解析并返回给客户端 
  *参数1：模板文件
  *参数2：给模板传递的参数 
    */
  
res.render('index',{
  title:'首页 ',
  content: 'hello swig'
});
```

4.开发过程中需要取消模板缓存的限制

```
swig.setDefaults({
 cache: false
});
app.set('view cache', false);
```

5.设置静态文件托管


```
// 当用户访问的是/public路径下的文件，那么直接返回
app.use('/public',express.static(__dirname + '/public'));
```
划分模块
- 前台模块
- 后台模块
- API模块


```
// 根据不同的功能划分模块
app.use('/',require('./routers/main'));
app.use('/admin',require('./routers/admin'));
app.use('/api',require('./routers/api'));
```
对于管理员模块 admin.js

```
var express = require('express');
var router = express.Router();
 
// 比如访问 /admin/user
router.get('/user',function(req,res,next) {
  res.send('User');
});
module.exports = router;
```

6.启动服务设置数据库的存储地址以及端口

```
var mongoose = require('mongoose');
// 数据库链接
mongoose.connect("mongodb://localhost:27017/blog",(err) => {
  if(err){
    console.log("数据库连接失败");
  }else{
    console.log("数据库连接成功");
   // 启动服务器，监听端口 
   app.listen(3000,(req,res,next) => {
      console.log("app is running at port 3000");
    });
  }
});
```
