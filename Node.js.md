#   Node.js

[TOC]



## 第一天



浏览器的运行环境

![image-20221130181938144](image\image-20221130181938144.png)

什么是node.js

![image-20221130182817515](image\image-20221130182817515.png)



### fs文件模块

![image-20221130200845506](image\image-20221130200845506.png)

**fs模块**

### **初始化require**

`const fs = require('fs')`

###读取文件 fs.readFile()语法格式

![image-20221130201219737](image\image-20221130201219737.png)

**代码：**

```js
//导入fs模块来操作文件
const fs = require('fs')
//调用fs.readFile()
// 参数1：读取文件的存放路径
//参数2：读取文件的编码格式
fs.readFile('./测试.txt', 'utf8', (err, dataStr) => {
    //如果成功err的值为null
    //如果失败则err的值为错误对象，data的值为undefined
    //打印失败结果
    console.log(err);
    console.log('----------------');
    // 打印成功结果
    console.log(dataStr);
})

```





### 向指定文件写入内容

**fs.writeFile()** 

![image-20221130205904879](image\image-20221130205904879.png)

```js
// 导入fs文件系统模块
const fs = require('fs')
//调用fs.writeFile()，写入文件内容
// 参数1：表示文件的存放路径
// 参数2：表示要写入的内容
// 参数3回调函数
fs.writeFile('./测试.txt', '陶正吃大便', (err) => {
    // 如果文件写入成功err=null
    console.log(err);
})

```





###路径问题

`__dirname`

```js
fs.readFile(__dirname + '/files/1.txt', 'utf8', function (err, dataStr) {
  if (err) {
    return console.log('读取文件失败！' + err.message)
  }
  console.log('读取文件成功！' + dataStr)
})

```



### Path路径模块

`const path = require('path')`

**路径拼接**

**`path.join()`**

![image-20221130233404326](image\image-20221130233404326.png)

**../会抵消前面一个路径**

> **注意**：**今后凡是涉及到路径拼接的操作，都要使用path.join（）方法进行处理**

```js
const path = require('path')
const fs = require('fs')
fs.readFile(path.join(__dirname, './files/1.txt'), 'utf8', (err, dataStr) => {
  if (err) return console.log(err.message);
  console.log('查找成功' + dataStr);

})

```





### 获取文件中的路径名

**`path.basename()`**

![image-20221130235353918](image\image-20221130235353918.png)

```js
const path = require('path')

// 定义文件的存放路径
const f = '/a/b/c/index.html'

//拿到整个文件
// const fullName = path.basename(f)

// console.log(fullName);


//去掉文件名的扩展名
const name = path.basename(f, '.html')
console.log(name);
```



### 获取路径中的文件扩展名

**`*path.extname()`**

![image-20221201000407231](image\image-20221201000407231.png)

```js
const path = require('path')

// 定义文件的存放路径
const f = '/a/b/c/index.html'

//拿到文件扩展名
const fext = path.extname(f)
console.log(fext);
```





### 第一天案例

需求：将一个html文件，分离css，js，html。

```js
//导入fs模块
const fs = require('fs')
const path = require('path')

//定义正则表达式
const regStyle = /<style>[\s\S]*<\/style>/
const regscript = /<script>[\s\S]*<\/script>/

//调用fs.readFile()方法读取文件
fs.readFile(path.join(__dirname, './index.html'), 'utf8', (err, dataStr) => {
    //判断调用是否成功
    if (err) return console.log('读取失败' + err.message);
    //css入口
    resolveCss(dataStr)
    //js入口
    resolvejs(dataStr)
    //html入口
    resobleHtml(dataStr)
})
//定义处理字符串的方法
function resolveCss(htmlStr) {
    //使用正则提取需要内容
    const r1 = regStyle.exec(htmlStr)
    //将提取出来的样式字符串，进行字符串replace替换，将style去除然后存入css文件中
    const newCss = r1[0].replace('<style>', '').replace('</style>', '')
    //将字符串写入对应css文件中
    fs.writeFile(path.join(__dirname, '../code/clock/index.css'), newCss, (err) => {
        if (err) return console.log('写入失败' + err.message);
        console.log('css文件写入成功');
    })
}

//定义js方法
function resolvejs(htmlStr) {
    const r1 = regscript.exec(htmlStr)
    const newJs = r1[0].replace('<script>', '').replace('</script>', '')
    fs.writeFile(path.join(__dirname, '../code/clock/index.js'), newJs, (err) => {
        if (err) return console.log('写入失败');
        console.log('js文件写入成功');
    })

}

//定义处理html方法
function resobleHtml(htmlStr) {
    //将字符串调用replace方法，把内嵌的style和script改为外联，此时就只剩下html结构，js和css都被替换掉了，成为了外联
    const newHtml = htmlStr.replace(regStyle, ' <link rel="stylesheet" href="./index.css">').replace(regscript, '<script src="./index.js"></script>')
    fs.writeFile(path.join(__dirname, '../code/clock/index.html'), newHtml, (err) => {
        if (err) return console.log('写入html失败');
        console.log('写入html成功');
    })

}
```













##               第二天





### http模块

![image-20221201100644048](image\image-20221201100644048.png)



**IP地址**

![image-20221201105320018](image\image-20221201105320018.png)



**域名**



![image-20221201105815573](image\image-20221201105815573.png)

**端口号：**

![image-20221201110032776](image\image-20221201110032776.png)



### 创建web服务器

#### ![image-20221201110318373](image\image-20221201110318373.png)

**创建http实力模块**

**创建web服务器实例**

调用 `http.createServer()`

绑定**`request`**事件

![image-20221201110608324](image\image-20221201110608324.png)



**启动服务器**

![image-20221201110716898](image\image-20221201110716898.png)



代码：

```js
//导入http模块
const http = require('http')
//创建web服务器实例
const server = http.createServer()
//为服务器实例绑定request事件，监听客户端请求                                                                           
server.on('request', function (req, res) {
    console.log('Someone visit our web server.');
})
//启动服务器
server.listen(80, function () {
    console.log('server running at http://127.0.0.1:80');
})
```



#### **req请求对象**

1. **req 是请求对象，包含了与客户端相关的数据和属性**

2. **req.url 是客户端请求的 URL 地址**

3. **req.method 是客户端请求的 method 类型**

   ![image-20221201140938331](image\image-20221201140938331.png)


```js
const http = require('http')
const server = http.createServer()
// req 是请求对象，包含了与客户端相关的数据和属性
server.on('request', (req, res) => {
  // req.url 是客户端请求的 URL 地址
  const url = req.url
  // req.method 是客户端请求的 method 类型
  const method = req.method
  const str = `Your request url is ${url}, and request method is ${method}`
  console.log(str)
  // 调用 res.end() 方法，向客户端响应一些内容
  res.end(str)
})
server.listen(80, () => {
  console.log('server running at http://127.0.0.1')
})

```





#### res响应对象

 **res.end(返回数据)**



![image-20221201150106504](image\image-20221201150106504.png)





#### **解决中文乱码问题**

`固定写法`

**`res.setH           eader('Content-Type', 'text/html; charset=utf-8')`**









### 模块化



**`模块化`是指解决一个复杂问题时候，自顶向下层把系统划分成若干模块的过程，对整个系统来说，`模块是可组合，分解和更换的单元`**

**把代码模块化的好处**

1. 提高了代码`复用性`
2. 提高了代码的`可维护性`
3. 可以实现`按需加载`



#### 模块作用域

**模块作用域和函数作用域类似，在自定义模块中的变量，方法等成员，只能在当前模块作用域内被发现，这种模块级别的访问限制叫做模块作用域**



#### 向外共享模块作用域中的成员





####module 对象



**在每个js自定义模块中都有一个module对象，它里面储存了和当前模块有关的信息**



**module.exports**



![image-20221201193949972](image\image-20221201193949972.png)

**exports 对象**

**默认情况下，`exports`和`module.exports`指向同一个对象，最终结果还是以module.exports指向为准**



**Commonjs模块化规范**

![image-20221202090736802](image\image-20221202090736802.png)







### npm与包

**什么是包**

![image-20221202090927936](image\image-20221202090927936.png)



**安装包的命令**

![image-20221202100921573](image\image-20221202100921573.png)



**安装指定版本的包**

![image-20221202110830953](image\image-20221202110830953.png)











## 第三天

### 包管理配置文件

![image-20221202111136955](image\image-20221202111136955.png)

**在上传时，剔除包文件**

![image-20221202112011900](image\image-20221202112011900.png)

**快速创建 package.json**

npm包管理工具提供了一个快捷命令，可以在执行命令所处的目录中，快速创建package.json这个包管理



**`npm init -y`**

可以执行命令所处目录中快速新建，package.json文件

![image-20221202113120911](image\image-20221202113120911.png)



**dependencies 节点**

记录安装的包

![image-20221202131521753](./image\image-20221202131521753.png)



#### 卸载包

**`npm uninstall`**

![image-20221202132355338](./image\image-20221202132355338.png)



**`devDependencies 节点`**

**如果某些包只在项目开发阶段用到，在项目上线之后不会用到则建议把这些包记录到devDependencies节点中**

用法：`npm i 包名 -D`

![image-20221202132808114](./image\image-20221202132808114.png)



### **切换npm下包镜像源**

![image-20221202134448492](./image\image-20221202134448492.png)

**nrm**

**通过npm包管理器，将nrm安装为全局可用的工具**

**`npm i nrm -g`**

**查看所有可用的镜像源**

**`nrm 1s`**

**将下包的镜像源切换为淘宝镜像**

**`nrm use taobao*`*

**包的分类**

![image-20221202135707867](./image\image-20221202135707867.png)





**全局包**

![image-20221202135925766](./image\image-20221202135925766.png)

安装与卸载

安装：**`npm i 包名 -g`**

卸载：**`npm uninstall 包名 -g`**

![image-20221202141440701](./image\image-20221202141440701.png)



**包结构**

![image-20221202142616914](./image\image-20221202142616914.png)



制作属于自己的包

package.json初始化

![image-20221202162326014](./image\image-20221202162326014.png)

**登录npm**

![image-20221202202444098](./image\image-20221202202444098.png)

**发包**

![image-20221202203216191](./image\image-20221202203216191.png)

**删除**

![image-20221202203827947](./image\image-20221202203827947.png)









### 模块加载机制

**模块在第一次加载后会被缓存，这也就意味着多次调用require（）不会被执行多次，不论是内置模块，用户自定义模块，还是第三方模块都会优先从缓存中加载，从而提高模块的加载效率** 

![image-20221202205544313](./image\image-20221202205544313.png)

**自定义模块加载机制**

![image-20221202210025230](./image\image-20221202210025230.png)

**第三方模块**

![image-20221202211759693](./image\image-20221202211759693.png)

![image-20221202212248434](./image\image-20221202212248434.png)









### Express

![image-20221202214516302](./image\image-20221202214516302.png)



**Express**

![image-20221202215156976](./image\image-20221202215156976.png)

**安装**

`npm i express@版本号`



### **监听GET请求**

`app.get()`

**app.get('请求URL'，functio(req,res){**

**})**

![image-20221202221843973](./image\image-20221202221843973.png)



### **监听post请求**

`app.post请求`

![image-20221202222018079](./image\image-20221202222018079.png)



### **把内容响应给客户端**

**可以把处理好的内容发送客户端**

`res.send()`

![image-20221202222458533](./image\image-20221202222458533.png)



### **req.query对象**

可以访问客户端通过查询字符串形式发送到服务器的参数

![image-20221203000135192](./image\image-20221203000135192.png)



### **获取URL的动态参数**

通过`req.params`对象，可以访问到URL中通过：匹配到的动态参数 

![image-20221203001647431](./image\image-20221203001647431.png)

**多个动态参数**

**`app.get('/user/:id/:name')`**

代码：

```js
const app = express()

// 4. 监听客户端的 GET 和 POST 请求，并向客户端响应具体的内容
app.get('/user', (req, res) => {
  // 调用 express 提供的 res.send() 方法，向客户端响应一个 JSON 对象
  res.send({ name: 'zs', age: 20, gender: '男' })
})
app.post('/user', (req, res) => {
  // 调用 express 提供的 res.send() 方法，向客户端响应一个 文本字符串
  res.send('请求成功')
})
app.get('/', (req, res) => {
  // 通过 req.query 可以获取到客户端发送过来的 查询参数
  // 注意：默认情况下，req.query 是一个空对象
  console.log(req.query)
  res.send(req.query)
})
// 注意：这里的 :id 是一个动态的参数
app.get('/user/:ids/:username', (req, res) => {
  // req.params 是动态匹配到的 URL 参数，默认也是一个空对象
  console.log(req.params)
  res.send(req.params)
})

// 3. 启动 web 服务器
app.listen(80, () => {
  console.log('express server running at http://127.0.0.1')
})

```



### Express 托管静态资源

`**express.static(),通过他我们可以非常方便的创建一个静态资源服务器**`

![image-20221203003534204](./image\image-20221203003534204.png)

**`app.use(express.static('文件夹路径'))`**



**挂载路径前缀**

![image-20221203005340242](./image\image-20221203005340242.png)



**nodemon**

![image-20221203005510014](./image\image-20221203005510014.png)

**安装nodemon**

![image-20221203005539609](./image\image-20221203005539609.png)



**使用**

**`nodemon .\文件名js`**

![image-20221203005753871](./image\image-20221203005753871.png)







## **第四天**

### 路由

**1.路由就是映射关系**



`**在Express中路由就是客户端的请求与服务器处理函数之间的映射关系，EXpress中路由3部分组成，分别是请求类型，请求url，处理吧函数**web`





#### 开发模式

1. **基于服务器端渲染的传统web开发模式**

2. **基于前后端分离的新型web开发模式**

   

   **服务器端的开发**

![image-20221203085534004](./image\image-20221203085534004.png)

**服务器端渲染的优缺点**



![image-20221203085610674](./image\image-20221203085610674.png)



**前后端分离的web开发模式**

前后端分离的概念：

![image-20221203090016141](./image\image-20221203090016141.png)·



**路由的匹配过程**

![image-20221203093837616](./image\image-20221203093837616.png)

**路由最简单的用法**

![image-20221203094744384](./image\image-20221203094744384.png)



#### **模块化路由**

为了方便对路由进行模块化管理，Expresas不建议将路由直接挂载到app上，而是**推荐将路由抽离为单独的模块**

![image-20221203095018350](./image\image-20221203095018350.png)

**`app.use()`**

**app.use()函数作用就是用来注册全局中间件**

![image-20221203102335506](./image\image-20221203102335506.png)



定义路由模块代码

```js
const express = require('express')
//创建路由对象
const router = express.Router()
router.get('/user/list', (req, res) => {
    res.send('陶正吃屎')
})

router.post('/user/add', (req, res) => {
    res.send('陶正吃大便')
})

module.exports = router
```

导入路由模块

```js
const express = require('express')
const app = express()
const router = require('./路由模块')
app.use(router)




app.listen(80, () => {
    console.log('http://127.0.0.1');

})
```



**为路由模块添加前缀**

**app.use('要添加的前缀', router)**









### 中间件

#### 中间件的概念

**中间件的调用流程**

![image-20221203104031868](./image\image-20221203104031868.png)

**中间件的格式**

![image-20221203104221843](./image\image-20221203104221843.png)



#### next函数

**next函数的作用**

![image-20221203104633446](./image\image-20221203104633446.png)



#### 定义中间函数

![image-20221203104914564](./image\image-20221203104914564.png)

代码：

```js
const express = require('express')
const app = express()


// 定义一个中间件函数
const nw = function (req, res, next) {
    console.log('这是一个最简单的中间件函数');
    //把下一个关系交给下一个中间件或者是路由
    next()

}



app.listen(80, () => {
    console.log('http://127.0.0.1');
})
```





**全局生效的中间件**

**客户端发起的任何请求，`到达服务器之后都会触发的中间件`，叫做全局生效的中间件**

![image-20221203110934024](./image\image-20221203110934024.png)

全局中间件代码：

```js
const express = require('express')
const app = express()


// 定义一个中间件函数
const nw = function (req, res, next) {
    console.log('这是一个最简单的中间件函数');
    //把下一个关系交给下一个中间件或者是路由
    next()

}
//将mw注册为全局生效的中间件
app.use(nw)
app.get('/', (req, res) => {
    res.send('Home page')
})
app.get('/user', (req, res) => {
    res.send('陶正吃粑粑')
})

app.listen(80, () => {
    console.log('http://127.0.0.1');
})
```



**全局中间件的简化形式**

![image-20221203112201612](./image\image-20221203112201612.png)

代码：

```js
const express = require('express')
const app = express()


app.use((req, res, next) => {
    console.log('这是最简单的中间件函数');
    next()

})



app.get('/', (req, res) => {
    res.send('Home page')
})
app.get('/user', (req, res) => {
    res.send('陶正吃粑粑')
})

app.listen(80, () => {
    console.log('http://127.0.0.1');
})
```





#### 中间件的作用

**多个中间件之间，共享同一份req和res基于这样的特性，我们可以在上游统一为 req或res对象添加自定义属性和方法，供下游中间件或者路由进行使用**

![image-20221203112833761](./image\image-20221203112833761.png)



#### **定义多个全局中间件**

**可以使用`app.use()`连续定义多个全局中间件，客户端请求到达服务器之后，会按照中间件定义先后顺序依次进行调用.**





#### **局部生效的中间件**

**不使用app.use()定义的中间件，叫做局部生效的中间件**

![image-20221203145323453](./image\image-20221203145323453.png)







**局部生效中间件代码**

```js
const express = require('express')
const app = express()


// 定义中间件函数
const mw1 = (req, res, next) => {
    console.log('调用了一次中间件函数');
    next()
}
// 局部生效中间件
app.get('/', mw1, (req, res) => {
    res.send('Home page')
})




app.get('/user', (req, res) => {
    res.send('傻逼陶正')
})


app.listen(80, () => {
    console.log('http://127.0.0.1');
})
```





#### 定义多个局部生效的中间件

![image-20221203150529184](./image\image-20221203150529184.png)

```js
// 局部生效中间件
app.get('/', mw1, mw2, (req, res) => {
    res.send('Home page')
})

```





#### 中间件使用的5个注意事项

> 1. **一定要在路由之前注册中间件**
> 2. **客户端发送过来的请求可以连续调用多个中间件进行处理**
> 3. **执行完中间件的业务代码之后，不要忘记调用next()函数**
> 4. **为了防止代码逻辑混乱，调用next()函数之后不要再写额外的代码**
> 5. **连续调用多个中间件时，多个中间件之间，共享req和res对象**



#### 中间件的分类

![image-20221203152310282](./image\image-20221203152310282.png)

**应用级别的中间件**

**通过`app.use()`或`app.get`或`app.post`，绑定到app实例上的中间件，叫做应用级别的中间件**

![image-20221203152555085](./image\image-20221203152555085.png)



**路由级别的中间件**

> **绑定到`express.Rputer()`实例上的中间件，叫做路由级别的中间件，它的用法和应用界别中间件没有区别。只不过，应用级别中间件是绑定到app实例上，路由级别中间件绑定到router实例上q**

![image-20221203152930167](./image\image-20221203152930167.png)





**错误级别中间件**

错误级别中间件用于捕获项目中发生的异常错误，从而防止项目异常崩溃的问题

格式：**错误界别的中间件中function处理函数中，必须有4个形参，形参顺序从前到后分别是（err，req，res，next）**

**`注意：错误中间件必须注册在所有路由之后`**



![image-20221203160040349](./image\image-20221203160040349.png)

```js
const express = require('express')
const app = express()

app.get('/', (req, res) => {
    //人为制造错误
    throw new Error('陶正正在吃屎')
    res.send('陶正爱吃大便')
})


//定义错误级别的中间件
app.use((err, req, res, next) => {
    console.log('发生了错误' + err.message);
    res.send('Error:' + err.message)
})

app.listen(80, function () {
    console.log('http://127.0.0.1');
})
```





#### Express内置的中间件

![image-20221203182343325](./image\image-20221203182343325.png)

#### **express.json中间件的使用**

**在服务器可以使用req.body这个属性来接收客户端发送过来的请求体数据不配置解析表单数据的中间件默认情况下，req.body等于undefined**

```js
const express = require('express')
const app = express()


//除了错误级别中间件，其他中间件必须在路由之前进行配置
app.use(express.json())

app.post('/user', (req, res) => {


    res.send('ok')
    console.log(req.body);
})
//在服务器可以使用req.body这个属性，来接收客户端发过来的请求体
//默认情况下如果不配置解析表单的中间件，则req.body默认等于underfined

app.listen(80, function () {
    console.log('http://127.0.0.1');
})
```





#### express.urlencoded 使用

**解析x-www-form-urlencoded**

**在服务器可以使用req.body这个属性，来接收客户端发过来的请求体**

```js
app.use(express.urlencoded({ extended: false }))
app.post('/user', (req, res) => {


    res.send('ok')
    console.log(req.body);
})
//在服务器可以使用req.body这个属性，来接收客户端发过来的请求体
//默认情况下如果不配置解析表单的中间件，则req.body默认等于underfined


app.post('/book', (req, res) => {
    res.send('okk')

    console.log(req.body);
})


```









**第三方的中间件**

![image-20221203193316392](./image\image-20221203193316392.png)





#### 自定义中间件

![image-20221203202658097](./image\image-20221203202658097.png)



封装：

```js
//导入node.js内置的querstring模块该函parse数专门用来处理查询字符串
const qs = require('querystring')


const bodyParser = ((req, res, next) => {
    //定义中间体具体业务逻辑
    //定义一个str字符串，专门用来存放客户端发送过来的请求体数据
    let str = ''
    //2.监听req的data事件
    req.on('data', (chunk) => {
        str += chunk
    })
    //监听req的end事件
    req.on('end', () => {
        //在str中存放的是完整的请求体数据
        const body = qs.parse(str)
        req.body = body
        next()
    })


})
module.exports = bodyParser
```



调用：

```js
const express = require('express')
const app = express()

//导入自己封装的中间件模块
const custom = require('./custom')

//将自定义中间件函数注册为全局可用的中间件

app.use(custom)

app.post('/user', (req, res) => {
    req.str = { "neme": 'fj', "age": 18 }
    res.send(req.str)
    console.log(req.body);
})


app.listen(80, () => {
    console.log('http://127.0.0.1');
})
```







### 使用express写接口

**配置：**

```js
const express = require('express')
const router = express.Router()


router.get('/get', (req, res) => {
    //通过req.query获取客户端通过查询字符串，发送到服务器的数据
    const query = req.query
    //调用res.send()方法，向客户端响应处理的结果
    res.send({
        status: 0, //0表示处理成功，1表示处理失败
        msg: 'GET请求成功!',
        data: query, //需要响应给客户端


    })
})
// 定义post接口
router.post('/post', (req, res) => {
    //通过req.body获取请求体中包含的url-encoded格式的数据
    const body = req.body
    res.send({
        status: 0,
        msg: 'POST',
        
        data: body
    })

})

// 将router暴露

module.exports = router
```



**调用：**

```js
const express = require('express')
const app = express()

// 如果要post就必须要配置解析表单数据的中间件
app.use(express.urlencoded({ extended: false }))


// 导入路由模块
const router = require('./apiRouter')

//把路由，模块注册到app上
app.use('/api', router)

app.listen(80, () => {
    console.log('http://127.0.0.1');
})
```







### 解决CORS跨域资源共享

![image-20221203224032589](./image\image-20221203224032589.png)

#### **使用cors中间件解决跨域问题**

![image-20221203224310614](./image\image-20221203224310614.png)

代码：

```js
 //一定要在路由之前配置cors这个中间件，从而解决接口跨域问题
const cors = require('cors')
app.use(cors())
```





**什么是CORS**

![image-20221203225924516](./image\image-20221203225924516.png)



**cors的注意事项**

![image-20221203230027332](./image\image-20221203230027332.png)





#### **cors响应头部**

![image-20221203234628532](./image\image-20221203234628532.png)



**2**

![image-20221203234652910](./image\image-20221203234652910.png)

**3**



![image-20221203234837553](./image\image-20221203234837553.png)



**4**

![image-20221203234931781](./image\image-20221203234931781.png)





#### CORS请求的分类

客户端在请求CORS接口时，根据请求方式和请求头的不同，可以将CORS请求分为两大类分别是：

1. `简单请求`
2. `预检请求`





##### 简单请求

![image-20221203235408856](./image\image-20221203235408856.png)

**预检请求**

![image-20221204000459807](./image\image-20221204000459807.png)



**简单请求和预检请求的区别**

![image-20221204000640799](./image\image-20221204000640799.png)



#### JSONP接口

![image-20221204001359269](./image\image-20221204001359269.png)









## 第五天

### 数据库

![image-20221205112415986](./image\image-20221205112415986.png)

### 常见的数据库以及分类

![image-20221205113210721](./image\image-20221205113210721.png)



### 数据库的基本概念

![image-20221205114754264](./image\image-20221205114754264.png)





![image-20221205115222464](./image\image-20221205115222464.png)





### 库表行字段的关系

![image-20221205115544765](./image\image-20221205115544765.png)



**组成**

![image-20221205153427372](./image\image-20221205153427372.png)







### 创建数据表

![image-20221205154735694](./image\image-20221205154735694.png)





### **Data Type 数据类型**

> 1. **int 整数**
> 2. **varchar（len）字符串**
> 3. **tinyint (布尔值)**

> **字段的特殊标识：**
>
> 1. **PK主键，唯一标识**
> 2. **NN值不允许为空**
> 3. **UQ值唯一**
> 4. **AI 值自动增长**



### 主界面的组成部分

![image-20221206094802211](./image\image-20221206094802211.png)



#### 使用SQL管理数据库

**SQL是结构化查询语言，专门用来访问和处理数据库的编程语言，我们以编程形式操作数据库里面的语言**

![image-20221206100925180](./image\image-20221206100925180.png)

SQL可以做什么

### ![image-20221206101029543](./image\image-20221206101029543.png)



### 重点掌握从SQL中

**查询数据，插入数据，更新数据，删除数据**

**额外掌握4种SQL语法**

**where条件，and和or运算符， order by排序，count函数**





### SQL语法

![image-20221206101658047](./image\image-20221206101658047.png)





**查询代码**

`select * from 数据库.数据表`



**查询指定**

![image-20221206113553457](./image\image-20221206113553457.png)



**代码：**

`select username，password from 数据库.数据表`



**SQL的 INSERT INTO 插入语句**

![image-20221206115105364](./image\image-20221206115105364.png)

`insert into new_sjk.users (username,passwored) values ('爱屎陶正',1234)`

![image-20221206140959864](./image\image-20221206140959864.png)





**SQLUPDATE语句**

**Update语句用于修改表中数据**

 new_sjk.users **数据库的工作簿**

password 是字段要更改的值

id是要更改的id



**`update new_sjk.users set passwored = '888888' where id = 7`**



**更新某一行里的若干列**

代码：

**`update new_sjk.users set passwored= 'admin123', status=1 where id=2`**



**删除 DELETE**

**DELETE 语句用于删除表中行**

![image-20221206152638336](./image\image-20221206152638336.png)



**DELETE 删除示例**

![image-20221206152820922](./image\image-20221206152820922.png)

代码：

`delete from new_sjk.users where id=1`



**SQL的where字句**

![image-20221206154214294](./image\image-20221206154214294.png)



**运算符**

![image-20221206154157288](./image\image-20221206154157288.png)





**SQL的AND和OR运算符**

![image-20221206154917182](./image\image-20221206154917182.png)

**and代码**

**`select * from new_sjk.users where id>1  and status！=1`**

**or代码**

**`select * from new_sjk.users where status = 1 or id<1`**



**ORDER BY 语句**

ORDER BY语句用于根据指定的列

![image-20221206163207492](./image\image-20221206163207492.png)



**升序排序**

![image-20221206163627741](./image\image-20221206163627741.png)

**`select * from new_sjk.users order by status`**

**降序desc**

`select * from new_sjk.users order by id desc`



**多重排序**

![image-20221206164354856](./image\image-20221206164354856.png)

**asc升序排序**

**`select * from new_sjk.users order by status desc,username asc`**



**COUNT(*)示例**

![image-20221206165652715](./image\image-20221206165652715.png)

代码：

`select count(*) from new_sjk.users where status=0`



**使用AS为列设置别名**

![image-20221206170213240](./image\image-20221206170213240.png)

**代码：**

**`select count(*) as total from new_sjk.users where status=0`**









### 在项目中操作MySQL

**在项目中操作MySQL**

![image-20221206170950070](./image\image-20221206170950070.png)



**链接数据库代码：**

```js
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1', //数据库的ip地址
    user: 'root', //数据库的账号
    password: 'admin123456', //登录数据库的密码
    database: 'new_sjk' //指定要操作的数据库

})
```



**安装与配置mysql模块**

![image-20221206174247215](./image\image-20221206174247215.png)

代码：

```js

const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1', //数据库的ip地址
    user: 'root', //数据库的账号
    password: 'admin123456', //登录数据库的密码
    database: 'new_sjk' //指定要操作的数据库

})

//测试mysql能否正常工作
db.query('select 1', (err, results) => {
     //mysql 模块工作期间报错了
    if (err) return console.log(err.message);
    // 能够成功执行sql语句
    console.log(results);
    

})
```







### 使用mysql模块操作MySQL数据库

#### 1.查询数据

```js
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1', //数据库的ip地址
    user: 'root', //数据库的账号
    password: 'admin123456', //登录数据库的密码
    database: 'new_sjk' //指定要操作的数据库

})

//查询users表中所有的用户数据
const sqlstr = 'select * from users'
//查询失败
db.query(sqlstr, (err, results) => {
    if (err) return console.log(err.message);
    //查询成功
    console.log(results);

})
```





#### **2.插入数据**

 ![image-20221206180312326](./image\image-20221206180312326.png)





代码：

```js
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1', //数据库的ip地址
    user: 'root', //数据库的账号
    password: 'admin123456', //登录数据库的密码
    database: 'new_sjk' //指定要操作的数据库

})
//向users表中，新增一条数据，其中username的值为spider password的值为pcc123
const user = { username: 'spider', passwored: 'pcc2123' }
// 定义待执行的sql语句
const sqlStr = 'insert into users (username,passwored) values (?,?)'
//执行sql语句
db.query(sqlStr, [user.username, user.passwored], (err, res) => {
    // 失败了
    if (err) return console.log(err.message);
    // 成功了
    //  如果执行的是insert into 插入语句，则res是一个对象
    //可以通过affectedRows来判断数据是否插入成功
    if (res.affectedRows === 1) return console.log('数据插入成功');
})
```









**插入数据的便捷方式**



代码：

```js
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1', //数据库的ip地址
    user: 'root', //数据库的账号
    password: 'admin123456', //登录数据库的密码
    database: 'new_sjk' //指定要操作的数据库

})
//插入数据便捷方式
const user = { username: 'sider2', passwored: 'pzz124' }
// 定义待执行的sql语句
const sqlStr = 'insert into users set ?'
// 执行sql语句
db.query(sqlStr, user, (err, res) => {
    if (err) return console.log(err.message);
    if (res.affectedRows === 1) return console.log('数据插入成功');
})
```





#### **3.更新数据**

![image-20221206191727955](./image\image-20221206191727955.png)

代码：

```js
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1', //数据库的ip地址
    user: 'root', //数据库的账号
    password: 'admin123456', //登录数据库的密码
    database: 'new_sjk' //指定要操作的数据库

})
//演示如何更新用户信息
const user = { id: 15, username: 'aaaa', passwored: '000' }
//定义sql语句
const sqlStr = 'update users set username=?,passwored=? where id=?'
//写入sql语句
db.query(sqlStr, [user.username, user.passwored, user.id], (err, res) => {
    if (err) return console.log(err.message);
    if (res.affectedRows === 1) {
        console.log('更新成功');
    }

})

```





**更新数据的简写方式**

![image-20221206194328519](./image\image-20221206194328519.png)



代码：

```js
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1', //数据库的ip地址
    user: 'root', //数据库的账号
    password: 'admin123456', //登录数据库的密码
    database: 'new_sjk' //指定要操作的数据库

})
//演示如何更新用户信息
const user = { id: 8, username: 'aaaaaa', passwored: '000' }
//定义sql语句
// set快捷
const sqlStr = 'update users set ? where id=?'
//写入sql语句
db.query(sqlStr, [user, user.id], (err, res) => {
    if (err) return console.log(err.message);
    if (res.affectedRows === 1) {
        console.log('更新成功');
    }

})

```



#### 4.删除数据

![image-20221206201931360](./image\image-20221206201931360.png)



```js
const mysql = require('mysql')
const db = mysql.createPool({
    host: '127.0.0.1', //数据库的ip地址
    user: 'root', //数据库的账号
    password: 'admin123456', //登录数据库的密码
    database: 'new_sjk' //指定要操作的数据库

})


// 删除id为5的用户
const sqlstr = 'delete from users where id=?'
db.query(sqlstr, 15, (err, res) => {
    if (err) return console.log(err.message);
    if (res.affectedRows === 1) console.log('删除成功');
})

```



#### 5.标记删除

![image-20221206203012439](./image\image-20221206203012439.png)

```js
// 标记删除
const sqlStr = 'update users set status=? where id=?'
db.query(sqlStr, [1, 7], (err, res) => {
    if (err) return console.log(err.message);
    if (res.affectedRows === 1) console.log('标记删除成功');



})
```













## 第六天

### 服务器端渲染的web开发模式

**服务器端渲染的概念：服务器发送给客户端的HTML页面，是服务器，通过字符串拼接动态生成的，因此客户端不需要使用Ajax这样的技术额外请求页面数据代码如下**

![image-20221206204603540](./image\image-20221206204603540.png)



**服务器端开发的优缺点**

**`优点`：`前端耗时少`，因为服务器负责动态生成HTML内容，浏览器只需直接渲染页面即可，尤其是移动端，更省电**

**`有利于SEO`。因为服务器响应的是完整的HTML内容，所以爬虫更容易获取信息有利于SEO**





### 前后端分离开发模式

**前后端分离概念：前后端分离的开发模式依赖于Ajax技术的广泛应用，后端只负责提供api接口，前端使用Ajax调用接口的开发模式**

优点：

> 1. `开发体验好。前端专注ui页面的开发，后端专注于api的开发，且前端有更多选择性`
> 2. `用户体验好，Ajax技术的广泛应用，极大的提高了用户体验，可以轻松实现页面局部刷新`
> 3. `减轻了服务器端的渲染压力，因为页面最终是在每个用户浏览器中生成的`

缺点：

**不利于SEO。因为完整的HTML需要在客户端动态拼接完成，所以爬虫无法爬取页面的有效信息，（解决方案：利用vue，react等前端框架的ssr技术能够很好的解决seo问题）**



### 如何选择开发模式

![image-20221206225206615](./image\image-20221206225206615.png)

### 身份认证

**什么是身份认证**

![image-20221206225332466](./image\image-20221206225332466.png)

**为什么需要身份认证**

**身份认证的目的是为了确认当前所声称为某种身份的用户，确实是所声称用户**

![image-20221206225700497](./image\image-20221206225700497.png)

### Session认证机制

**http的无状态性**

![image-20221206225906124](./image\image-20221206225906124.png)

**如何突破HTTP的无状态限制**

![image-20221206230115934](./image\image-20221206230115934.png)

### 什么是Cookie

**Cookie是储存在用户浏览器中的一段不超过4kb的字符串，`它由一个名称，一个值和其他几个用于控制Cookie有效期，安全性，使用范围的可选属性组成`**

![image-20221206230654021](./image\image-20221206230654021.png)



**Cookie在身份认证中的作用**

**1.客户端第一次请求服务器时候，服务器通过响应头的形式，向客户端发送一个身份认证的Cookie客户端会自动将Cookie保存在浏览器中**

**2.当客户端每次请求服务器时候，浏览器会自动将身份认证相关的Cooke，通过请求头的形式发送给服务器服务器可验明客户端身份**

![image-20221206231226971](./image\image-20221206231226971.png)



### Cookie不具有安全性

> **由于Cookie是储存在浏览器中的，而且浏览器也提供了读写Cookie的Api，因此Cookie很容易被伪造，不具有安全性，因此不建议将重要的隐私数据通过Cookie的形式发送给浏览器**

![image-20221206231756754](./image\image-20221206231756754.png)

**提高身份认证的安全性**

![image-20221206232011986](./image\image-20221206232011986.png)

### Session的工作原理

![image-20221206232745730](./image\image-20221206232745730.png)



### 配置express-session中间件

![image-20221206232823214](./image\image-20221206232823214.png)



**从sessiong中获取数据**

![image-20221206235246228](./image\image-20221206235246228.png)



**清空sessiong**

![image-20221206235546695](./image\image-20221206235546695.png)



代码：

```js
// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// TODO_01：请配置 Session 中间件
const session = require('express-session')
app.use(
  session({
    secret: 'itheima',
    resave: false,
    saveUninitialized: true,
  })
)

// 托管静态页面
app.use(express.static('./pages'))
// 解析 POST 提交过来的表单数据
app.use(express.urlencoded({ extended: false }))

// 登录的 API 接口
app.post('/api/login', (req, res) => {
  // 判断用户提交的登录信息是否正确
  if (req.body.username !== 'admin' || req.body.password !== '000000') {
    return res.send({ status: 1, msg: '登录失败' })
  }

  // TODO_02：请将登录成功后的用户信息，保存到 Session 中
  // 注意：只有成功配置了 express-session 这个中间件之后，才能够通过 req 点出来 session 这个属性
  req.session.user = req.body // 用户的信息
  req.session.islogin = true // 用户的登录状态

  res.send({ status: 0, msg: '登录成功' })
})

// 获取用户姓名的接口
app.get('/api/username', (req, res) => {
  // TODO_03：请从 Session 中获取用户的名称，响应给客户端
  if (!req.session.islogin) {
    return res.send({ status: 1, msg: 'fail' })
  }
  res.send({
    status: 0,
    msg: 'success',
    username: req.session.user.username,
  })
})

// 退出登录的接口
app.post('/api/logout', (req, res) => {
  // TODO_04：清空 Session 信息
  req.session.destroy()
  res.send({
    status: 0,
    msg: '退出登录成功',
  })
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(80, function () {
  console.log('Express server running at http://127.0.0.1:80')
})

```



### **jwt**

![image-20221207000849456](./image\image-20221207000849456.png)

![image-20221207003525974](./image\image-20221207003525974.png)

### JWT工作原理

![image-20221207003704783](./image\image-20221207003704783.png)



### JWT组成部分

**`JWT组成部分分别是：Header（头部），payload（有效载荷），Signature（签名）`**

![image-20221207003949805](./image\image-20221207003949805.png)

![image-20221207004019314](./image\image-20221207004019314.png)







### JWT的使用方式

![image-20221207095404171](./image\image-20221207095404171.png)

### 在项目中使用jwt

![image-20221207095747624](./image\image-20221207095747624.png)

代码

```js
// TODO_01：安装并导入 JWT 相关的两个包，分别是 jsonwebtoken 和 express-jwt
const jwt = require('jsonwebtoken')
const expressJWT = require('express-jwt')

```



### 定义secret密钥

![image-20221207104117793](./image\image-20221207104117793.png)

代码：

```js

//定义secretKey
const secretKey = 'fj123'
```



### 登陆后生成JWT字符串

![image-20221207105132522](./image\image-20221207105132522.png)





### 将jwt字符串还原为json对象

![image-20221207112021057](./image\image-20221207112021057.png)



### 捕获jwt失败后产生的错误

![image-20221207144703241](./image\image-20221207144703241.png)























## **第九天**

### ES6模块化

#### 模块化分类

![image-20221207202757014](./image\image-20221207202757014.png)

**什么是ES6模块化规范**

ES5模块化规范中定义：

> 1. **每个js文件都是一个独立的模块**
> 2. **导入其他模块成员使用import关键字**
> 3. **向外共享模块成员使用export关键字**

![image-20221207203530231](./image\image-20221207203530231.png)

### ES6模块化的基本语法

**ES6模块化主要包含如下三种语法**

> 1. **默认导出与默认导入**
> 2. **按需导出与按需导入**
> 3. **直接导入并执行模块中的代码**

![image-20221207203859210](./image\image-20221207203859210.png)



**默认导入语法**

**`默认导入语法：import 接收名称 from '模块标识符'`**



![image-20221207204250278](./image\image-20221207204250278.png)

导出代码：

```js
let n1 = 10
let n2 = 12
function fn() {

    console.log(11);
}
export default {
    n2,
    fn
}
```



**导入代码**

```js
import m1 from './默认导出.js'
console.log(m1);
```







### 默认导出的注意事项

> **`1.每个模块中只允许使用唯一的一次 export default 否则会报错`**
>
> **`2.默认导入时接收的名称可以任意命名，只要是合法成员名称即可，不能以数字开头`**
>
> **`3.导出文件要记得加后缀名*`*

### 按需导出与按需导入

**按需导出**

![image-20221207205731564](./image\image-20221207205731564.png)

代码：

```js
export let f = 10
export let n = '陶正吃屎'
export function fn() {
    return '陶正大便'
}
```



**按需导入**

![image-20221207205719822](./image\image-20221207205719822.png)

```js
import { n, fn } from './默认导出.js'
console.log(n);
console.log(fn());
```



### 按需导出与按需导入的注意事项

![image-20221207210823919](./image\image-20221207210823919.png)

![image-20221207212521009](./image\image-20221207212521009.png)



### Promise

> **Promise是一个构造函数**
>
> - `我们可以创建Promise的实例const p = new Promise`
> - `new出来的promise实例对象，代表一个异步操作`
>
> 2**.Promise.prototype上包含一个.then()方法**
>
> - `每一次new Promise()构造函数得到的实例对象`
> - `都可以通过原型链方式访问到.then()方法，例如p.then`
>
> 3.**.then()方法用来预先指定成功和失败的回调函数**
>
> - `p.then(成功的回调函数，失败的回调函数)`
> - `p.then(res=>{},err=>{})`
> - `调用.then()方法时，成功的回调函数是必选的，失败的回调函数是可选的`

### promise状态的改变

![image-20221209141356733](./image\image-20221209141356733.png)

### promise的基本流程

![image-20221209141645963](./image\image-20221209141645963.png)

### 回调地狱



多层回调函数的相互嵌套就形成了回调地狱。示例代码如下

![image-20221208141839970](./image\image-20221208141839970.png)



### then-fs的基本使用

![image-20221208142218471](./image\image-20221208142218471.png)

上述代码无法保证文件的读取顺序需要做进一步改进

****



**then()方法的特性**

![image-20221208145309885](./image\image-20221208145309885.png)

代码：

```js
import thenFs from 'then-fs'

thenFs.readFile('./one.txt', 'utf8')
    .then((r1) => {
        console.log(r1);
        return thenFs.readFile('./two.txt', 'utf8')
    })
    .then((r2) => {
        console.log(r2);
    //readFile会返回一个新的promise实例对象，，如果上一个。then方法中返回了promise实例对象，则可以通过下一个then处理
        return thenFs.readFile('./there.txt', 'utf8')
    })
    .then((r3) => { console.log(r3); })


```





### 通过.catch捕获错误

![image-20221208153117761](./image\image-20221208153117761.png)

**注意：.catch如果放到了最后那么发送错误后面的。then则不会执行**

**如果需要执行则可以把.catch放到可能发生错误的.then下**

![image-20221208153847322](./image\image-20221208153847322.png)

### promise.resovle方法

1.**如果传入的参数非promise类型的对象，则返回的结果为成功的promise对象**

**2.如果传入的参数为promise对象，则参数结果决定了resolve的结果**



### promise.reject方法

**返回一个失败的promise对象无论传递的是什么都返回一个失败的promise对象**



###util.promisify方法



Node.js 内置的 `util` 模块有一个 [`promisify()` 方法](https://link.juejin.cn/?target=https%3A%2F%2Fnodejs.org%2Fapi%2Futil.html%23util_util_promisify_original)，该方法将基于回调的函数转换为基于 Promise 的函数。这使您可以将 Promise 链和 `async/await` 与基于回调的 API 结合使用。

```js
// 封装读取文件
import util from 'util'
import fs from 'fs'
// 调用方法返回一个promise
let mine = util.promisify(fs.readFile)
mine('./two.txt').then(value => {
    console.log(value.toString());
})
```



### Promise.all()方法

![image-20221208154234440](./image\image-20221208154234440.png)



### Promise.race方法

![image-20221208155112535](./image\image-20221208155112535.png)



### **封装自己的读取文件**

```js
import fs from 'fs'

function getFile(fpath) {
    //接收文件并且返回一个Promise实例对象
    // res和rej就是then的两个预先调用的回调函数 分别对应 then('成功回调，失败回调)用户调用then
    //方法就是实例传给了函数形参再将结果返回给用户
    return new Promise(function (res, rej) {
        fs.readFile(fpath, 'utf8', (err, datastr) => {
            //判断是否发送错误将err传给then失败回调函数，rej就是then的失败回调函数
            if (err) return rej(err)
            // 如果没有就返回成功结果
            res(datastr)
// 这两个分别是then回调函数实参传给函数形参，再通过这两个传递回给then
        })
    })

}
//用户调用方法传递文件
getFile('./one.txt')
    .then((r1) => {
        console.log(r1);
    })
    .catch(err => { console.log(err.message); })
```





### async/await

**什么是async/await**

![image-20221208162606157](./image\image-20221208162606157.png)



使用async/await

![image-20221208163145860](./image\image-20221208163145860.png)

代码：

```js
import thenFs from 'then-fs'


//如果有await则必须有 async修饰
// async修饰
async function getAll() {
    const r1 = await thenFs.readFile('./one.txt', 'utf8')
    console.log(r1);

    const r2 = await thenFs.readFile('./there.txt', 'utf8')
    console.log(r2);

}
getAll()
```





**`如果有await则必须有 async修饰`**





### async/await的使用注意事项



![image-20221208164714576](./image\image-20221208164714576.png)

### Promise.prototype.final（）



**`final（）`** 方法返回一个 [`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)。在 promise 结束时，无论结果是 正确 或者是 错误，都会执行指定的回调函数。这为在 是否成功完成后都需要执行的代码提供了一种方式。这避免了同样的语句需要在 [`then（）`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) 和 [`catch（）`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) 中各写一次的情况。`Promise`







### 同步任务和异步任务

![image-20221208165906465](./image\image-20221208165906465.png)

###![image-20221208170023885](./image\image-20221208170023885.png)

同步任务和异步任务的执行过程

![image-20221208165747376](./image\image-20221208165747376.png)

### EVentLoop的基本概念

**`Javascript主线程从任务队列中读取异步任务的回调函数，放到执行栈中依次执行，这个过程是循环不断的，所以整个的这种运行机制又叫EventLoop`**

![image-20221208171655088](./image\image-20221208171655088.png)

### 宏任务和微任务

**什么是宏任务和微任务**

> 1**.宏任务**
>
> - **异步Ajax请求**
> - **setTimeout，setinterval**
> - **文件操作**
> - **其他宏任务**
>
> **2.微任务**
>
> - **promise.then,.catch和finally**
> - **process.nextTick**
> - **其他微任务**

![image-20221208172603497](./image\image-20221208172603497.png)

### 宏任务和微任务的执行顺序

![image-20221208172832275](./image\image-20221208172832275.png)

  
