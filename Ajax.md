#  Ajax

## 第一天

### 服务器

服务器指对外提供资源的电脑

**URL地址概念**

URL中文叫同意资源定位符，用于表示互联网上每个资源存放地址·   

**URL地址组成部分**

URL地址一帮由三部分组成

1. 客户端与服务器之间的通信协议
2. 存有该资源的服务器名称
3. 资源在服务器上具体的存放位置

**图例：**

![image-20221121130201078](image\image-20221121130201078.png)

**图解客户端与服务器的通信过程**

![image-20221121131044552](image\image-20221121131044552.png)





### 请求数据

**数据也是一种资源**

**`XMLHttpRequest`对象**

如果要请求数据，则需要用到`XMLHttpRequest`对象（简称xhr）是浏览器提供的js成员，通过他可以请求服务器上的资源

**用法：** `let xhrObj =  new XMLHttpRequest()`



**资源的请求方式**

客户端请求服务器最常见的两种方式分别为get和post

- get请求通常用于获取服务器资源

例如：根据url地址，从服务器获取HTML文件，css文件，js文件，图片文件，数据资源等

- post请求通常用于向服务器提交数据（往服务器送资源）

例如：登陆时向服务器提交登录信息。注册信息，添加用户向服务器提交的用户信息等各种数据提交操作



### 了解Ajax

Ajax的全称是ASynchronous Javascript And XML (异步JavaScript和XML)

通俗理解就是：在网页中利用XMLHttpRequest对象和服务器进行数据交互的方式就是Ajax  

![image-20221121133402554](image\image-20221121133402554.png)



### JQuery中的Ajax

jQuery中发起Ajax请求最常用 的方法如下

- `$.get()`
- `$.post()`
- `$.ajax()`



#### $.**get**()

**语法：**

`$.get(url,[data],[callback])`

**被中括号包起来的代表是可选参数不是数组**

![image-20221121134143842](image\image-20221121134143842.png)

**$.get()发起不带参数的请求**

![image-20221121134345885](image\image-20221121134345885.png)

**代码：**

```js
$(function(){
$('#btn').on('click',function(){
    $.get('http://www.liulongbin.top:3006/api/getbooks',function(res){
        console.log(res);
    })
})
})

```

**$.get()发起带参数的请求**

图例：![image-20221121140720363](image\image-20221121140720363.png)

**代码：**

```js
$(function(){
$('#btn').on('click',function(){
    $.get('http://www.liulongbin.top:3006/api/getbooks',{id:1},function(res){
        console.log(res);
    })
})
})

```



#### $.post()

语法：

$`.post(url,[data],[callback])`

![image-20221121141042170](image\image-20221121141042170.png)

代码：

```js
$('#btn').on('click',function(){
    $.post('http://www.liulongbin.top:3006/api/addbook',{bookname:'陶正',author:'今年19',publisher:'爱吃'},function(res){
        console.log(res);
    })
})
```



#### $.ajax()函数语法

```js
语法：

$.ajax({

type:'',//请求的发过誓，例如GET或POST
url:'',//请求的url地址
data：{},//这次要携带的数据
success:function(res){}//请求成功后的回调函数

})

```

ajax发起get请求

```js
$(function(){
$('#btn').on('click',function(){
   $.ajax({
    type:'get',
    url:'http://www.liulongbin.top:3006/api/getbooks',
    data:{
        id:1
    },
    success:function(res){
        console.log(res);
    }
    
   })

})
})
```

$.ajax发起数据请求

```js
$(function(){
$('#btn').on('click',function(){
   $.ajax({
    type:'POST',
    url:'http://www.liulongbin.top:3006/api/addbook',
    data:{
       name:'3333',
       age:'113',
       da:23

    },
    success:function(res){
        console.log(res);
    }
    
   })

})
})
```





### 根路径代码

![image-20221126210604931](image\image-20221126210604931.png)

```js
$.ajaxPrefilter(function (options) {
    options.url = 'http://www.liulongbin.top:3007' + options.url
})
```



#### 接口

使用ajax请求数据是被请求的url地址就叫做数据（简称接口），同时每个接口必须有请求方式



![image-20221121171135412](image\image-20221121171135412.png)

![image-20221121171310859](image\image-20221121171310859.png)

**接口测试工具**

接口测试工具能让我们在不写任何代码情况下对接口进行调用和测试

![image-20221121183022976](image\image-20221121183022976.png)



![image-20221121183950749](image\image-20221121183950749.png)



#### 接口文档

一个合格的接口的接口文档应该包含以下六项内容

1. **接口名称**：用来标识各个接口的简单说明，如**`登录接口，获取图书列表接口`**等
2. **接口URL**：接口的调用地址
3. **调用方式**：接口的调用方式，如`**GET或者POST**`
4. **参数格式**：接口需要传递的参数，每个参数必须包含**`参数名称`**，**`参数类型`**。**`是否必选`**，**`参数说明`这四项内容**
5. **响应格式**：接口的返回值的详细描述，一般包含**`数据名称，数据类型，说明`**等三项内容
6. **返回实例**（可选）：通过对象的形式，距离服务器返回的数据结构

![image-20221121185208348](image\image-20221121185208348.png)

![image-20221121185240968](image\image-20221121185240968.png)

![image-20221121185303832](image\image-20221121185303832.png)



## 第二天



### **form表单的基本使用**

表单在网页中主要是采集功能

![image-20221121233329721](image\image-20221121233329721.png)



**`**action**`**

1. action属性规定提交表单时，**向何处发送表单**
2. action 属性的值应该是后端提供的一个**URL地址**，这个URL地址专门负责表单提交过来的数据
3. 当form表单在未指定action属性值的情况下，**action的默认URL地址**

`注意：当表单提交后页面会立即跳转到action指定的地址`



`**target属性**`

target属性用来规定在何处打开 **`action URL`**

![image-20221121234619431](image\image-20221121234619431.png)



`**method属性**`

method属性用来规定是以何种方式把表单体提交到action URL 它的值有两个，分别是get和post

默认情况下method的值为get，表示通过	URL地址的形式，把表单数据提交到action URL

![image-20221121235408431](image\image-20221121235408431.png)





`**enctype**`

![image-20221122083220140](image\image-20221122083220140.png)

**注意：如果涉及到文件上传时，必须将enctype的值设置为`multipart/form-data`**

****如果表单提交不涉及文件上传操作，则直接将enctype的值设置为**`application/x-www**`

`**-form-urlencoded`即可**



**表单同步提交的缺点**

1. `form表单提交后会发生跳转跳转到action URL`
2. `form表单同步提交后页面之前的状态和数据会丢失`

**解决方案：表单只负责采集数据，ajax负责将数据提交到服务器**



**阻止表单默认提交行为**

调用事件对象：

**`e.preventDefault`**()



快速获取表单中的数据

**1.serialize()函数**

$(selector).serialize

serialize()函数的好处“可以一次性获取到表单中所有数据

![image-20221122085749522](image\image-20221122085749522.png)

**注意：在使用serialize()函数快速获取表单数据是，必须为每个表单添加name属性**



### 模板引擎

**根据程序员指定的模板结构与数据自动生成一个完整的HTML页面**

![image-20221122140219448](image\image-20221122140219448.png)

![image-20221122141910701](image\image-20221122141910701.png)

**使用模板引擎初始化代码**

```js
<div></div>
//模板引擎：定义模板
//模板的HTML结构必须要定义到script中
<script type="text/html" id="tpl-user">
 // 双大括号代表要填充数据的地方
  <div>{{name}} ---- {{age}}----{{}}</div>
</script>

  <script>
//定义模板所需要的数据
    var data = {
      title: '<h3>用户信息</h3>',
      name: '张三',
      age: 20,
      isVIP: true,
      regTime: new Date(),
      hobby: ['吃饭', '睡觉', '打豆豆']
    }
    // 模板会有一个返回值
let str = template('tpl-user',data)
// 打印
document.querySelector('div').innerHTML=str
  
```



![image-20221122191937748](image\image-20221122191937748.png)

![image-20221122200323919](image\image-20221122200323919.png)

```js
 <div>
    {{@title}}
  </div>
```

**模板引擎中的判断**

```js
<script type="text/html" id="tpl-user">
<!-- 模板引擎里的判断 -->
  <div>{{name}} ---- {{age}}----{{}}</div>
  <div>{{if flag === 0}}
    flag是0
    {{else if flag ===1}}
    flag 是1
    {{/if}}
  </div>
  
</script>
```



**循环语法**

![image-20221122201612338](image\image-20221122201612338.png)

```js
  <div>
    {{each hobby}}
    {{$index}}{{$value}}
    {{/each}}
  </div>
```

![image-20221122203342082](image\image-20221122203342082.png)

**过滤器语法：**

{{value|filterName}}

![image-20221122203622684](image\image-20221122203622684.png)

![image-20221122204251727](image\image-20221122204251727.png)

**管道语法**

```js
 <script type="text/html" id="tpl-user">
{{time|dateFormat}}
//script里的入口函数
template.defaults.imports.dateFormat =(date)=>{
     console.log(date);

}
```



### **正则与字符串**

**基本语法**

exec()函数用于检索字符串中的正则表达式匹配

如果字符串中有匹配值则返回匹配值，否则返回null





**分组**

![image-20221123000606402](image\image-20221123000606402.png)

字符串中replace函数

返回新值

![image-20221123000943833](image\image-20221123000943833.png)

封装简易模板

```js
<div id="user-box"></div>

  <script type="text/html" id="tpl-user">
    <div>姓名：{{name}}</div>
    <div>年龄：{{ age }}</div>
    <div>性别：{{  gender}}</div>
    <div>住址：{{address  }}</div>
  </script>

  <script>
    function temp(id, data) {
      let str = document.getElementById(id).innerHTML
      let pattern = /{{\s*([a-zA-Z]+)\s*}}/

      let pat = null
     // 每一次获取到的都是，[0{{xx}}，1，{{1}}]
      while (pat = pattern.exec(str)) {
        str = str.replace(pat[0], data[pat[1]])//相当于data[pat[name]]
      }
      return str
    } 
    var data = { name: 'zs', age: 28, gender: '男', address: '北京顺义马坡' }

    // 调用模板引擎
    var htmlStr = temp('tpl-user', data)
    document.getElementById('user-box').innerHTML = htmlStr
  </script>
```

------















## 第三天

### XMLHttpRequest的基本使用

#### 使用xhr发起GET请求

![image-20221123083649645](image\image-20221123083649645.png)

 **代码**

```js
   // 1. 创建 XHR 对象
    let xhr = new XMLHttpRequest()
    // 2. 调用 open 函数
    xhr.open('get', 'http://www.liulongbin.top:3006/api/getbooks')
    // 3. 调用 send 函数
    xhr.send()
    // 4. 监听 onreadystatechange 事件
    xhr.onreadystatechange = function () {
      // 获取服务器响应的数据
      //这个判断条件是一个固定写法
      if (xhr.readyState === 4 && xhr.status===) console.log(xhr.responseText);
    }
```



**HML对象的reayState属性**

HMLHttpRequest对象的reayState属性，用来表示当前ajax所处的请求状态，每个ajax请求必然是一下状态中一个

![image-20221123085004957](image\image-20221123085004957.png)

**用xml发起带参数的请求**

![image-20221123085441255](image\image-20221123085441255.png)

```js
 let xhr = new XMLHttpRequest()
    xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=1')
    xhr.send()
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4 && xhr.status === 200) console.log(xhr.responseText);
    }
```



**查询字符串**

![image-20221123102244303](image\image-20221123102244303.png)

URL编码与解码

**什么是url编码**

特殊字符例如中文就会转化为url编码每3个%为一个字

![image-20221123131318806](image\image-20221123131318806.png)

**解码与编码**

代码：

```js
  // 编码
        let str = '陶正爱吃屎'
        let str2 = encodeURI(str)
        console.log(str2);

        //解码

        let str4 = decodeURI('%E9%99%B6')
        console.log(str4)
```

注：浏览器会自动对url地址进行编码大多数情况下程序员不需要关系url地址编码与解码操作

#### 使用xhr发起post请求 

步骤：

1. 创建**xhr**对象
2. 调用**`xhr.open`**()函数
3. 设置**`Content-Type`**属性(**固定写法**)
4. 调用**`xhr.send()`**函数，**同时指定要发送的数据**
5. 监听**`xhr.onreadystatechange`**事件

代码：

```js
  //post请求
        //创建shr对象
        let xhr = new XMLHttpRequest()
        // 调用open函数
        xhr.open('POST', 'http://www.liulongbin.top:3006/api/addbook')
        // 设置content-type属性
        // 固定写法
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
        // 调用send函数
        xhr.send('bookname=陶正&author=爱吃屎&publisher=上海出版社')
        // 监听事件
        xhr.onreadystatechange = () => {
            if (xhr.readyState === 4 && xhr.status === 200) console.log(xhr.responseText);
        }
```

------





### 数据交换格式

**数据交换格式就是服务器端与客户端之间数据传输与交换的格式**

**前端领域经常提及的两种数据交换格式分别是XML和JSON**

------



#### **XML**

**什么是XML**

XML英文全称是EXtensible Markup Language 即可扩展标记语言与HTML类似是一种标记语言

![image-20221123134915913](image\image-20221123134915913.png)

![image-20221123135025168](image\image-20221123135025168.png)

#### JSON

**什么是json**

json的英文全称是**`J`ava`s`cript `O`bject `N`otation** 即**对象表示法**，简单来**`讲JSON就是Javascript对象和数组的字符串表示法`**，它使用一个js对象或是数组信息，因此**`JSON的本质是字符串`**

**作用**

json是一种轻量级的文本数据交换格式，作用上类似于xml专门用于数据传输和储存，但比xml更小更快更易解析



**JSON的两种结构**

**`对象结构`**

> **在JSON中所有对象结构数据必须用双引号包裹**
>
> {key：valu，key2：value，key3：value} 键值对结构
>
> 其中value的数据类型可以是：
>
> 1. 数字
> 2. 字符串
> 3. 布尔值
> 4. null
> 5. 数组
> 6. 对象

![image-20221123135835385](image\image-20221123135835385.png)

**`数组结构`**

> 数组结构在json中表示为[]包裹起来的内容，数据结构为[”java““javascript”，30，true]
>
> 数组结构类型可以是：
>
> 1. 数字
> 2. 字符串
> 3. 布尔值
> 4. null
> 5. 数组
> 6. 对象

![image-20221123140908052](image\image-20221123140908052.png)

#### **JSON语法的注意事项**

> 1. **属性名必须用双引号包裹**
>
> 2. **字符串类型的值必须用双引号包裹**
>
> 3. **JSON中不允许使用单引号表示字符串**
>
> 4. **JSON中不能写注释**
>
> 5. **JSON的最外层必须是对象或数组的格式**
>
> 6. **不能使用undefined或函数作为JSON的值**
>
>    
>
>    
>
>    **JSON的作用：**
>
>    在计算机与网络之间储存和数据传输
>
>    **JSON的本质：**
>
>    用字符串来表示javascript对象数据或数组数据





### 序列化和反序列化

**`序列化`：把数据对象转换为字符串的过程叫做序列化，例如调用JSON.stringify()函数的操作，叫做JSON序列化**

**`反序列化`：把字符串转换为数据对象的过程叫做反序列化，例如调用JSON.parse()函数的操作叫做JSON反序列化**

**一键将表单值序列化：`表单.serialize()`**

****

### 封装ajax函数

```js
// 转化查询字符串
function resolveData(data) {
    let arr = []
    // 循环目的是把用户传输的对象转化为k=value&k=valu格式
    for (let k in data) {
        let str = `${k}=${data[k]}`
        arr.push(str)
    }
    return arr.join('&')
}
// 接收用户传回来的值,一个类似于$ajax的格式
// $.itheima({

//     type: '',//请求的方式，例如GET或POST
//     url: '',//请求的url地址
//     data：{},//这次要携带的数据
//     success: function (res) { }//请求成功后的回调函数

// })
function itheima(options) {
    let xhr = new XMLHttpRequest()
    //将用户给的值转换为查询字符串内
    let qs = resolveData(options.data)
    //判断
    //toUpperCase()强制转化大写method=get&&post
    if (options.method.toUpperCase() === 'GET') {
        //url根目录qs是用户输入的值被转换为查询字符串
        xhr.open(options.method, options.url) + '?' + qs
        //向服务器传输参数在send里传输qs
        xhr.send()
    } else if (options.method.toUpperCase() === 'POST') {
        //请求方式以及地址
        xhr.open(options.method, options.url)
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
        xhr.send(qs)
    }
    // 监听返回的事件
    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
            // 将返回的值转化反序列化
            let result = JSON.parse(xhr.responseText)
            //返回给用户请求成功后的回调函数
            options.success(result)
        }
    }

}
```





### XMLHttpRequest  Level2

1. 可以设置HTTP的请求时限
2. 可以使用FormData对象管理表单数据
3. 可以上传文件
4. 可以获取数据传输的进度信息



**设置HTTP请求时限**

**`xhr.timeout = 3000`**

将最长等待时间设置为3000毫秒，过了这个时间就自动停止HTTP请求

还有一个配套的timeout时间，用来指定回调函数

xhr.ontimeout = function(e){

alert('请求超时')

}

**代码**

```js
 let xhr = new XMLHttpRequest()
    xhr.timeout = 30
    xhr.ontimeout = function () {
        console.log('请求超时');
    }
```





**FormData属性管理对象的基本使用**

  `let fd = new FormData()`

 fd.append('uname', 'zs')

代码：

```js
 // 1. 创建 FormData 实例
    let fd = new FormData()
    // 添加
    fd.append('uname', 'zs')
    fd.append('age', '18')
    let xhr = new XMLHttpRequest()
    xhr.open('POST',
             'http://www.liulongbin.top:3006/api/formdata')
    xhr.send(fd)
    xhr.onreadystatechange = (res) => {
      if (xhr.readyState === 4 && xhr.status === 200) {
        console.log(JSON.parse(xhr.responseText));
      }
    }

```



**直接获得FormData表单的数据**

**代码**： **`let fd = new FormData(form)`**

```js
 // 1. 通过 DOM 操作，获取到 form 表单元素
    var form = document.querySelector('#form1')

    form.addEventListener('submit', function (e) {
      // 阻止表单的默认提交行为
      e.preventDefault()

      // 创建 FormData，快速获取到 form 表单中的数据
      let fd = new FormData(form)
      let xhr = new XMLHttpRequest()
      xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata')
      xhr.send(fd)
      xhr.onreadystatechange = (res) => {
        if (xhr.readyState === 4 && xhr.status) {
          console.log(JSON.parse(xhr.responseText));
        }
      }

    })
```



### **上传文件**

1. 定义Ul结构
2. 验证否文件(属性`files`)
3. 向FormData追加文件
4. 使用xhr发起上传文件的请求
5. 监听onreadystatechange

```JS
 <!-- 1. 文件选择框 -->
  <input type="file" id="file1" />
  <!-- 2. 上传文件的按钮 -->
  <button id="btn">上传文件</button>
  <br />
  <!-- 3. img 标签，来显示上传成功以后的图片 -->
  <img src="" alt="" id="img" width="800" />
  <div class="progress">
    <div class="progress-bar progress-bar-striped active" id="Complete" role="progressbar" aria-valuenow="45"
      aria-valuemin="0" aria-valuemax="100" style="width: 0%">
      <span class="sr-only">45% </span>
    </div>
  </div>
  <script>
    // 1. 获取到文件上传按钮
    let btn = document.querySelector('#btn')
    // 2. 为按钮绑定单击事件处理函数
    btn.addEventListener('click', function () {
      let files = document.querySelector('#file1').files
      if (files.length <= 0) {
        return alert('11')
      }
      // 将用户选择的文件，添加到 FormData 中
      let fd = new FormData()
      fd.append('avatar', files[0])
      //上传
      let xhr = new XMLHttpRequest()
      //监听文件上传进度
      xhr.upload.onprogress = function (e) {
        if (e.lengthComputable) {
          let time = Math.ceil((e.loaded / e.total) * 100)
          console.log(time);
          document.querySelector('#Complete').style.width = `${time}%`




        }
      }




      xhr.open('POST', 'http://www.liulongbin.top:3006/api/upload/avatar')
      xhr.send(fd)
      // 上传成功
      // 监听事件
      xhr.onreadystatechange = () => {
        if (xhr.readyState === 4 && xhr.status === 200) {
          let data = JSON.parse(xhr.responseText)
          if (data.status === 200) {
            document.querySelector('#img').src = 'http://www.liulongbin.top:3006' + data.url
          } else {
            console.log('图片上传失败' + data.message);
          }
        }
      }

    })
    

```









**使用Ajax上传文件**



代码：

```js
<body>

  <input type="file" id="file1" />
  <button id="btnUpload">上传文件</button>

  <br />
  <img src="./images/loading.gif" alt="" style="display: none;" id="loading" />

  <script>
    $(function () {
       // 当监听到Ajax请求之后执行函数
      $(document).ajaxStart(function () {
        $('#loading').show()
      })
      // 当检测到ajax请求结束之后执行操作
      $(document).ajaxStop(function () {
        $('#loading').hide()
      })
      
      // 定义点击上传文件
      $('#btnUpload').on('click', function () {
        // 将用户传的文件保存到files里
        let files = $('#file1')[0].files
        // 判断条件用户是否上传文件
        if (files.length <= 0) {

          return alert('请选择文件')
        }
        // 创建一个表单元素实例
        let fd = new FormData()
        // 将用户上传的文件存到fd中
        fd.append('avatar', files[0])
        // 发起ajax请求
        $.ajax({
          method: 'post',
          url: 'http://www.liulongbin.top:3006/api/upload/avatar',
          data: fd,
          // 只要是上传文件必须要指定这两个值为false
          processData: false,
          contentType: false,
          success: function (res) {
            console.log(res);
          }
        })


      })


    })
  </script>
```

![image-20221124131239639](image\image-20221124131239639.png)



**监听Ajax请求**



**ajaxStart**

```js
// 当监听到Ajax请求之后执行函数
      $(document).ajaxStart(function () {
        $('#loading').show()
      })
```

**ajax请求结束**

**ajaxStop**

```js
 // 当检测到ajax请求结束之后执行操作
      $(document).ajaxStop(function () {
        $('#loading').hide()
      })
```

**jquery规定监听ajax必须写在documen中**





`### a`xios

**axios发起get请求**

**axios.get()**

![image-20221124141108080](image\image-20221124141108080.png)

代码：

```js
 document.querySelector('#btn1').addEventListener('click', function () {
      let url = 'http://www.liulongbin.top:3006/api/get'
      let par = { name: 'sfa', age: 18 }
      axios.get(url, { params: par }).then(res => {
        console.log(res.data);
      })
    })
```



**axios发起POST请求**

![image-20221124141427802](image\image-20221124141427802.png)



代码：

```js
 //发起axiosPOST请求

    document.querySelector('#btn2').addEventListener('click', function () {
      let url = 'http://www.liulongbin.top:3006/api/post'
      let dataObj = { address: '北京', location: '顺义区' }
      axios.post(url, dataObj).then(res => {
        console.log(res);
      })
    })
```





使用axios发起请求

![image-20221124142235946](image\image-20221124142235946.png)

**发起get请求代码**

```js
document.querySelector('#btn3').addEventListener('click', function () {
      let url = 'http://www.liulongbin.top:3006/api/get'
      let par = { name: '钢铁侠', age: 35 }
      axios({
        menthod: 'get',
        url: url,
        params: par,
      }).then(res => {
        console.log(res);
      })
    })

```



**发起post请求**

```js
document.querySelector('#btn4').addEventListener('click', function () {
      let url = 'http://www.liulongbin.top:3006/api/post'
      let dataObj = { address: 'saf', location: 'asf顺义f区' }
      axios({
        method: 'post',
        url: url,
        data: {
          name: '娃哈哈',
          age: 18,
          gender: '女'
        }
      }).then(res => {
        console.log(res);
      })
    })
```



------







## 第四天

### 同源策略

**同源**

#### **如果两个页面的协议，域名和端口都相同，则两个页面具有相同的源**

![image-20221124151957464](image\image-20221124151957464.png)

**协议：http：**

**域名：www.xxx.com**

**端口：默认80端口和7001端口**

![image-20221124152519053](image\image-20221124152519053.png))

#### 跨域

**同源**`是连个url的协议域名端口一致`**反之则是跨域**

![image-20221124152901208](image\image-20221124152901208.png)

![image-20221124153609480](image\image-20221124153609480.png)



**如何实现跨域请求**

![image-20221124153819845](image\image-20221124153819845.png)



### JSONP

JSONP是JSON的一种使用模式，可以用于解决主流浏览器的跨域数据访问的问题

![image-20221124154110161](image\image-20221124154110161.png)

![image-20221124154646435](image\image-20221124154646435.png)



JSONP原理



**通过调用服务器端的指定的接口**

**callback**指定调用的数据

查询字符串的形式调用值就是函数

```js
<script src="xxx/jsonp？callback=fn/"></script>
```

代码：

```js
   function abc(data) {
      console.log('JSONP响应回来的数据是：')
      console.log(data)
    }
  </script>

  <script src="http://ajax.frontend.itheima.net:3006/api/jsonp?callback=abc&name=ls&age=30"></script>
```

**JSONP的缺点**

> 只支持GET请求不支持POST**请求**
>
> JSONP和Ajax没有任何关系，不能把JSONP请求称为Ajax因为没有XMLHttpRequest这个对象



​	jQuery里的JSONP

JQuery里提供的$.ajax除了可以发起Ajax还可以发起JSONP数据

![image-20221124164728542](image\image-20221124164728542.png)

代码：

```js
  $(function () {
      // 发起JSONP的请求
      $.ajax({
        url: 'http://ajax.frontend.itheima.net:3006/api/jsonp?name=zs&age=20',
        // 代表我们要发起JSONP的数据请求
        dataType: 'jsonp',
        jsonp: 'callback',
        jsonpCallback: 'abc',
        success: function (res) {
          console.log(res)
        }
      })
    })
```



**自定义参数以及回调函数名称**

![image-20221124165318309](image\image-20221124165318309.png)

发送到服务器端默认值是callback

**jsonp：'callback'**

自定义回调函数的名称，默认值为jQuery格式

 **自定义参数**
  **jsonp: 'cb'**

**自定义参数回调参数名称**

**jsonpCallback：'abc'**

```js
 $(function () {
      // 发起JSONP的请求
      $.ajax({
        url: 'http://ajax.frontend.itheima.net:3006/api/jsonp?address=北京&location=顺义',
        //调用方式jsonp
        dataType: 'jsonp',
        // 自定义参数
        jsonp: 'cb',
        // 自定义回调参数名称
        jsonpCallback: 'abc',
        success: function (res) {
          console.log(res);
        }
      })
    })
```



**JSONP在jQuery中的实现过程**

![image-20221124170534486](image\image-20221124170534486.png)

### 搜索框代码以及节流的应用

```js
 // 节流函数
    $(function () {
      let times
      let obj = {}
      //kw是ketwords，把用户输入的值给节流函数在在时间到了之后把值传给ajax通过jsonp请求淘宝链接
      //把淘宝链接给的数据通过模板引擎渲染到页面，
      function debounceSearch(kw) {
        times = setTimeout(() => {
          getsuggestList(kw)

        }, 500)

      }


      //获取用户输入的值
      //键盘弹起清空定时器

      $('.ipt').on('keyup', function () {
        let ketwords = $(this).val().trim()
        // 如果没有获取到值则清空列表
        if (ketwords.lenght <= 0) return $('#suggest-list').empty().hide()
        clearTimeout(times)
        // 缓存判断本地有没有数据有数据直接渲染·
        if (obj[ketwords]) return rander(obj[ketwords])
        debounceSearch(ketwords)
      })


      //  提交ajax jsonp
      function getsuggestList(kw) {
        $.ajax({
          //kw是用户输入的关键字
          url: 'https://suggest.taobao.com/sug?q=' + kw,
          dataType: 'jsonp',
          success: res => {
            console.log(res);
            rander(res)
          }


        })
      }
      // 渲染
      function rander(res) {
        if (res.result <= 0) return $('#suggest-list').empty().hide()
        //  有值的话清空表单再渲染
        //将值传入模板引擎遍历后渲染
        let str = template('tpl-suggestlist', res)
        $('#suggest-list').html(str).show()
        // 缓存
        // 添加值
        // 获取用户输入的内容当作键
        obj[$('.ipt').val().trim()] = res
        console.log(obj);
      }



    })

```





### 节流防抖

![image-20221125010415692](image\image-20221125010415692.png)

**节流代码**

```js
  $(function () {
      // 1. 获取到图片
      let img = $('#angel')
      // 步骤1. 定义节流阀
      let timer = null
      // 2. 绑定 mousemove 事件
      $(document).on('mousemove', function (e) {


        console.log(`未延时`);
        if (timer) { return }
        // 步骤3：判断节流阀是否为空
        timer = setTimeout(() => {
          $(img).css('left', e.pageX + 'px').css('top', e.pageY + 'px')
          timer = null


        }, 16)
      })
      // 3. 设置图片的位置
      // 步骤2：开启延时器

    })
```











## 第五天





### HTTP

**通信的主题是服务器和客户端浏览器**

![image-20221125083542453](image\image-20221125083542453.png)





**通信协议**

通信协议是指通信双方通信所必须遵守的规则和约定

![image-20221125084015752](image\image-20221125084015752.png)

**HTTP协议**

- 客户端要以HTTP协议要求的格式把数据提交给服务器
- 服务器要以HTTP协议要求的格式把内容响应给客户端



![image-20221125084400841](image\image-20221125084400841.png)





#### **HTTP请求消息**



**由于HTTP协议属于客户端浏览器和服务器之间的通信协议，因此，客户端发起的请求叫做HTTP请求，客户端发送到服务器的消息，叫做HTTP请求消息**

**注意：HTTP消息又叫做请求报文**



**HTTP的请求消息组成部分**

**HTTP请求消息由，`请求行，请求头部，空行，和请求体`，四个部分**

![image-20221125084855774](image\image-20221125084855774.png)



**请求行**

**请求行由`请求方式，URL和HTTP协议版本`3个部分组成**这三者之间由空格隔开

![image-20221125085353849](image\image-20221125085353849.png)



**请求头部**

**请求头部用来描写客户端**

![image-20221125085547521](image\image-20221125085547521.png)





![image-20221125085614336](image\image-20221125085614336.png)





![image-20221125085953596](image\image-20221125085953596.png)



**空行**

最后一个请求字段后面是空行，通知服务器请求头部至此结束

请求消息中的空行用来分隔请求头部与请求体



**请求体**

**请求体中存放的，是需要通过POST方式提交到服务器的数据**

![image-20221125101515288](image\image-20221125101515288.png)



**注意**：**只有POST请求，GET请求没有请求体**



#### HTTP响应消息



**响应消息就是服务器响应给客户端的消息内容也叫做响应报文**



**HTTP响应消息的组成部分**

**HTTP响应消息由状态行，响应头部，空行，和响应体**

![image-20221125105651253](image\image-20221125105651253.png)



**状态行**



![image-20221125113729499](image\image-20221125113729499.png)



**响应头部**

响应头部是用来描述服务器的基本信息的，响应头部由多行键/值对组成键值对之间用英文冒号分开



![image-20221125121431987](image\image-20221125121431987.png)

   **空行**

**在最后一个响应头部结束之后，会紧跟一个空行，用来通知客户端响应头部自此结束**

![image-20221125121918537](image\image-20221125121918537.png)

**响应体**

![image-20221125122009067](image\image-20221125122009067.png)





#### HTTP请求方法

![image-20221125122141764](image\image-20221125122141764.png)

![image-20221125122526924](image\image-20221125122526924.png)



### **HTTP响应状态码**

**HTTP响应状态码也属于HTTP协议的一部分，`用来标识响应状态`**



![image-20221125123046179](image\image-20221125123046179.png)



### 响应状态码的组成以及分类

![image-20221125123259778](image\image-20221125123259778.png)

#### 2**成功相关的响应状态码

![image-20221125123428286](image\image-20221125123428286.png)

#### 3**重定向相关的响应状态码

![image-20221125123639955](image\image-20221125123639955.png) 

#### 4**客户端错误相关的响应状态码

![image-20221125124222844](image\image-20221125124222844.png)

#### 5**服务器端错误相关的响应状态码

![image-20221125124508161](image\image-20221125124508161.png)
