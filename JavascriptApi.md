##  &IJavasrcipt Api

### 声明变量

`const`
声明变量（不变）
Dom对象
标签在标签里是标签标签在js里是对象
选择第一个单个
`document.querSelector（‘css选择器’）`
选择多个
`domcument.querselectorAll（ul li）`



### 操作dom元素

**不解析标签**
`inner.Text`
**改变文字解析标签**
`innerHTML`

**图片**
对象.属性
`img.src`

**操作元素属性样式**

**对象.style.样式**

**元素.className = css类名**



**追加切换删除**

**追加一个类**
`**元素.classList.add（'‘类名’）**`
**删除一个类**
**`元素.classList.remove（‘类名’）`**
**切换一个类，无则追加，有则删除**
**`元素.classList.toggle(类名)`**
**替换一个类**
**`div.classList.replace(被替换，替换)`**
**查询包含某个类**
`**log（div.classList.contains('类名)）**`



#### 操作表单属性

**获取表单文字**
`表单.value=‘’`
表单.type= 属性

表单选择
`input.属性 = 布尔值`

**自定义属性**
**类名以data开头**
`dataset（获取以deta开头的所有属性）`
`box.dataset.id（获取deta对象中的id类名）`



### 计时器函数

**间歇函数**
`**setInterval`**（函数/匿名函数，间隔时间（毫秒））如果调用全局函数则不添加小括号
每一个间歇函数都有自己的值可以通过返回值的方法得到
`let n = setInterval(fn 100)`
关闭**

`clearInterval（变量名）**`

**延时函数**

**`setTimeout`(回调函数，等待毫秒数)**
**只执行一次**
**清除延时函数**
**`let timer = serTimeout(回调函数，等待毫秒数)`**
**`clearTimeout（timer）`**





### 事件

**点击事件**

> `对象.addEventListener（'click，function（）{}）`

**鼠标经过事件**

> `对象.addEventListener（‘mouseenter’，functiom，（）{}）`

**有冒泡的鼠标经过事件**

> `对象.addEventListener('mouseover',funtion(){} )`

**获取焦点**

> `对象.addEventListener('focus',finction(）{})`

**失去焦点**

> `对象.addEventListerner('blur',function(){})`

**键盘按下**

> `对象.addEventListener('keydown'functiom(){})`

**键盘弹起**

> `对象.addEventListener('keyup'function(){})`

**用户输入文本事件**

> `对象.addEventListener('input',function(){})`

**鼠标按下**

> `对象.addEventListener('mousedown',函数)`

**鼠标弹起**

> `对象.addEventListener('mouseup',函数)`

**鼠标移动**

> `对象.addEventListener('mousemove',函数)`

**change事件，鼠标失去焦点获取文本框**

> `对象.addEventListener('change'functiom(){})`

**表单事件**

> `对象.addEventListener('input'functiom(){})`

**鼠标事件对象**

pageX
pageX

> `对象.addEventListener('change'functiom(e){`
>
> `e`.pageX()
>
> e.pageX
>
> `})`

#### 事件对象

**获取事件对象**
**谁触发就指向谁一般用e因为这是规范**

```js
div.addEventListener（'click，function（e）{
}）
```

**部分常用属性**
**获取当前事件类型**
`type`
**获取光标相对于浏览器可见窗口的左上角位置**
`clientX/clienY`
**获取光标相对于当前DOM元素左上角位置**
`offsetX/offsetY`
**用户按下键盘的值**
`key kyd`
**键盘码**
**keycoode**

```js
键盘输入
 对象.addEventListener('keyup',function(e){
    console.log(e.key);
  })
```

#### **清除空格**

**去除两侧空格**
`属性.value.trim（）`



#### `this环境对象`

**this代表当前函数运行所处环境**
**谁调用this就指向谁**



### 事件流

**什么情况会发生事件流？**

**子元素与父元素设置了同样的事件**

**事件捕获**

**从网页到父标签到子元素（较少使用）**
`document - 父元素 -子元素`

**事件冒泡**

**从子元素比到网页**
`子元素 - 父元素 -document`

**阻止事件冒泡**

**阻止事件冒泡需要拿到事件对象**

语法：**`事件对象.stopPropagation()`**

代码：

```js
对象.addEventListener（'click，function（e）{
e.stopPropagation()
```





### 事件解绑

L0事件解绑

```js
btn.onclick = function（）{
alert（‘’）}
```

**L1**

```js
对象.removeEventListener（事件类型，事件函数）
```



### 事件委托

**原理：利用事件冒泡给父元素设置事件，子元素通过触发事件触发父元素事件**
`设置触发父元素事件时子元素收到事件`

代码：

```js
对象.addEventListener（'click，function（e）{
判断只对某一种子元素生效（target . tagName就是我们点击的那个对象 ，可以获得真正的触发事件）
if（e.target.tagName == ‘LI’）（注意tagName，获取的都是大写）{
e.target.style.colo = ‘red’
}
```



### 阻止元素默认行为

语法：`e.preventDefault`()

代码

```js
对象.addEventListener（'click，function（）{
e.preventDefault()
}）
```





### 页面加载事件

**load**

```js
window.addEventListener('load',function()){
}
```

**DOMcontentLoaded**

```js
document.addEventListener('DOMcontentLoaded',function(){})
无需等待图片样式表完全加载，html标签加载完成就解析

```



### 页面滚动事件（scroll）

监听页面滚动
**window**

代码：

```js
window.addEventListener（‘scroll’，function（{
执行的操作
}））
```

**滚动事件**

`scroll`

```js
div.addEventListener（'scroll’，function（）{
div.scrollTOP
}）

```



### **页面被卷去的距离**

**头部**

`**scrillTop**`

```js
window.addEventListener（'scroll’，function（）{
this.scrollTOP
}）
```

**滚动到指定坐标**

语法：**元素.scrollTo（x,y）**

### **页面尺寸事件**

`resize`

```js
window.addEvebtListener(‘resize’,function(){})
```

 **获取页面可见的部分宽高（不包含边框，margin，滚动条）**

`**clientWidth和clientHeight**`

语法：**对象.clientWidth**

**获取相对元素位置**

**会获取元素自身宽高，包含自身奢姿的宽高，padding。border**
`**offsetWidth和offsetHeight`**

**获取位置（注意是可读属性，获取元素距离自身定位父级元素的左上距离）**

**`offsetLeft和offsetTop`**

**获取元素位置**

**返回元素大小以及相对是视口位置**

`element.getBoundingClientRect()`



### **让滚动条添加滑动效果**

```css
css效果
html{
  scroll-behavior: smooth;
}
```





### 时间戳

**得到当前时间**

**实例化**

`const = date = newDate（）`

**指定时间**

`const date1 = new Date('2022-10-1 08:30:00')`

**日期对象的方法**

const date = new Date()

**方法**

`**const date = new Date()**`

**获得年份**

`date.getFullYear()`

**获得月份（0-11需要加一）**

`dete.getMonth()+1`

**获得月份中的每一天**

`date.getDate()`

**获取星期（0-6 零代表星期天）**

`date.getDay()`

**获取小时**

`dete.getHours()`

**获得分钟（0-59）**

`**dete.getMinutes()**`

**获得秒（0-59）**

`dete.getSeconds()`

**其他简写方式**

**div.innerHTML = date.toLocaleString**

**获取时间戳的几种方式**

**const date = new  Date()**

**1.getTime** (只能获得当前时间戳)
**`date.getTime()`**

**2.+new Date()**
**`+new Date()`**
**3.Date.now**
**`Date.now()`**

> **转化公式**
> d=parselnt(总秒数/60/60/24)day
> h = parsenint（总秒数/60/60%24）house
> m = parsenint（总秒数/60%60）min
> s=parseint（总秒数%60）second

### DOM节点

**查找父节点**

`子元素.parentNode`

**查找子节点**

`父元素.childNodes`

**获得所有子节点包括文本节点（空格换行）。注释**

`父元素.children`

**仅获得所有元素节点返回一个伪数组**

`父元素.children`

**下一个兄弟节点**

`nextElementSibling`

**上一个兄弟节点**

`previousElementSibling`

### 创建节点

#### **创建节点**

`document.createElement（'标签名）`

**插入节点**

**插入到父元素的最后一个子元素**
`父元素.appendChild(要插入的元素)` 

**插入到某个子元素的前面**

`父元素.insertBefore（要插入的元素，在那个元素前面）`

#### 复制节点

**克隆一个节点**

`元素.cloneNode（布尔值）`

> 若为true则会包含后代节点一起克隆
> **若为false则代表克隆时不包含后代节点**
> 默认为false

#### 删除节点

`父元素.removeChild(要删除的元素)`
注；如果不不存在父子关系则删除不成功



### m端事件（移动端）

**对象.addEventListener(m端事件，函数)**

**触摸**
`touchstart`
**离开**
`touchend`
**移动**
`touchmove`

`touchend`

当用户释放触摸时执行 JavaScript（仅适用于触摸屏）：

**重置事件**
`元素.reset()`
`this.reset()`



### 事件循环（event loop）

> **1.主线程不断重复获得任务执行任务的过程称为事件循环，异步任务完毕推入任务队列中，主线程在去获得任务**
> js的执行机制
> **2.执行站先执行同步任务在去，当有异步任务是提交对应的异步进程处理器（ajax，Dom事件，计时模块）**

### location对象

**默认跳转**

`location.href`

**属性**

获取地址中?携带的参数符号后面部分

`location.search`

**哈希值**

获取#号后面哈希值

`location.hash`

**刷新**

`location.reload`

强制刷新reload（true）

### 对象navigator

> 1.通过userAgent检测浏览器及其平台
> `navigator.userAgent`
> 2.前进后退
> `对象.history.go（）+1/-1`

### 本地存储分类

#### localStorage

**数据存储**

**`localStorage.setltem(key,value)`**

**数据获取**

**`对象(localStorage.getItem('uname'）`**

**数据删除** 

**`对象localStorage.removeltem(key)`**

**属性名都要加引号，值不需要加引号**

**临时储存**

**`sessionStorage`**

> 1. **特性生命周期为关闭窗口吧**
> 2. **在同一个窗口下数据可以共享**
> 3. **以键值对形式存储**
> 4. **用法跟localStorage基本相同**

### 储存复杂数据类型

**复杂数据类型必须转换为JSON字符串储存**

**JSON对象 属性和值都有引号，而且引号统一是双引号**

**转字符串储存用法：**
`localStorage.setItem('obj',JSON.stringify(obj))值不需要加引号`

**JOSN字符串转对象用法**
`JSON.parse（localStorage.getItem('obj')）`

### 数组中的map方法（映射函数）

**迭代数组**

**作用：map迭代数组，遍历，处理数据并且得到新的数组。**

代码：

```js
arr.map（function(item,i){

}）(item得到数组元素i是索引下标)

```

### **join（）方法**

> 1. **join（）方法用于把数组中所有元素转换成字符串语法**
> 2. **arr.join（‘’）括号内可以为空也可以为分割符号**





### 正则表达式（RegularExpression）

**作用**

1. **表单验证（匹配）**
2. **过滤敏感词（替换）**
3. **字符串中提取我们想要的部分（提取 ）**

**正则分为两步**

1. 定义规则
2.  检测匹配



**判断test**

**`regObj.test（被检测的字符串）`**
方法用来查看指定字符串是否匹配
找到返回true否则返回false
注意会被空格影响

**检索查找**

**`查找exec（）`**

找到返回一个数组，未找到返回一个null
语法：**`rag.exec（被检查的字符串）`**

**元字符**

元字符26英文字母写法
[a-z] 

**边界符**

> 1. ^定义以某个字符串开头
> 2. $表示行尾表示以匹配的文本进行结尾
> 3. ^1$精确匹配必须以他开头以他结尾



**量词**

> 1. *重复零次或更多次
> 2. +重复一次或者更多次
> 3. ？重复零次或一次
> 4. {n}重复n次
> 5. {n,}重复n次或者更多次
> 6. {n,m}重复n-m次



**字符类**

> 只能选择一个
> （/^[a-z]$/.test('')）
> （/^[a-z]{2}$/.test('')）
>
> ^写在中括号里面的叫取反
> [取反]
> .除了换行之外的任何字符 



**预定义**

> 1. \d匹配0-9之间任意数字相当于0-9
> 2. \D匹配0-9以外字符，相当于[……^0-9]
> 3. \w匹配任意字母数字下划线相当于[A-Za-z0-9]
> 4. \W除任意字母数字下划线相当于[A-Za-z0-9]
> 5. \s匹配空格（包括换行符号，副表符，空格符号）
> 6. \S匹配非空格符号
> 7. . 匹配任何任意字符  例如 .  可以匹配 1，n，*，+，- ,等

**修饰符**

> 1. i正则匹配时候不区分大小写
> 2. g匹配所有满足正则表达式的结果
> 3. /表达式/i

**查找替换**

> 1. replace替换
> 2. 字符串. replace（/正则表达式/，替换的文本）

[^0-9]:

**命名捕获分组**

![image-20230108133903843](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108133903843.png)

**反向断言和正向断言**

![image-20230108133804585](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108133804585.png)



