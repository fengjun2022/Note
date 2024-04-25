# jQuery



### **入口函数**

方法一：`$(function(){})`

方法二`jQuery(function(){})`

> $同时也是jquery是顶级对象

### **获取jQuery对象**

`$('div)`

DOM对象转jquer对象

1.直接获取`$('video')`

**jquery对象转dom对象**

index`是索引号

``$（'div）[index]`

`$（‘div’）.get（index）`

**jquery基础选择器**
`$('css选择器')`

> $('css选择器).css('属性'，属性值 )

**jQuery 筛选选择器** 

- ：first  用法 `$（li:first）`获取第一个li元素

- ：last   用法  `$("i:last")`取最后一个li元素

- ：eq（index）用法 `$("li:eq（2）"` 获取数组元素中索引号为2的

- ：odd  `$("li:odd")` 获取到li元素中选择索引号为奇数的元素

- ：even `$（“li：even”）`获取 li的元素，选择索引号偶数的元素

**筛选方法**

- parent（） 用法          `$（"li"）parent` 查找父级

- childeren (selector)   `$("ul")children("li")` 相当于$("ul">"li")查找亲儿子

- find(selector)         `$("ul")find("li")` 相当于$（“ul li”）后代选择器

- siblings（selector）      `$("first)siblings("li")` 查找兄弟节点不包括本身

- nextAll（[expr]）          `$("first)nextAll()` 查找当前元素之后的所有元素

- prevtAll（[expr]）      `$(".last").prevAll ()` 查找当前元素之前的所有同辈元素

- hasClass（class）        `$('div').hasClass ("protected")` 检查当前元素是否有某个特定的类如果有则返回true

- eq（index）    $("li").eq(2) 相当于 `$("li:eq"),index`

指定父级选择器
`parents（'选择器'）`

**鼠标经过事件**

`show` 显示
`hide`  隐藏

代码：

this是当前jquery元素 children选择子元素 show显示

显示

```js
$("nav>li").mouseover（function）（）{
$(this).children("ul").show()
}
```

隐藏

```js
$("nav>li").mouseout（function）（）{
$(this).children("ul").hide()
}
```

**jQuery的排他思想**

代码：

```js
$(function(){

    $('button').click(function(){
        $(this).css('backgroundColor','red')
        $(this).siblings('button').css('backgroundColor','')
    })
})
```

**获得当前索引号**

`let ndex = $（this）.index()i`

**链式编程**

作用：节约代码量

代码：

```js
$(this).css(`backgroundColor`,'red').siblings().css('backgroundColor','')

```

**操作css的用样式方法**

`$(this).css（‘color’，‘red）`属性名返回属性值



**参数对象形式**

```js
$(this).css({
"color" = '',
width = '
})
```



**设置类的样式方法**

### 添加类

`$("div").addClass('current)`

删除类

`$('div').removeClass('current')`

切换类

`$('div')toggle.addClass('current')`



### **jQuery显示隐藏动画效果**

显示 `show（[speed],[easing],[fnll]）`

隐藏  `hide[speed],[easing],[fnll]）`

1. speed :三种速度之一的字符串（show，nomal.or.fast）

2.  easing：（optional）用来切换指定效果默认是swing，可用参数linear

3. fn ：回调函数在动画完成时候执行函数

   

**滑动效果**

下滑动语法

`alideDown（[speed],[easing],[fnll]）`

上拉

`slideUp（[speed],[easing],[fnll]）`

切换

`slideToggle   （[speed],[easing],[fnll]）`



**事件切换**

hover

1. over鼠标经过函数就要触发的函数
2. out鼠标离开时触发

**停止排队**

`stop()`

1. stop方法用于停止你动画效果 
2. stop写到动画效果的前面相当于结束上一次动画
   必须写在动画前面

代码

```js
$(".nav>li").hover(function(){
         $(this).children('ul').stop().slideToggle()
         })

```



**淡入效果**

`fadeIn([speed],[easing],[fn])`

**淡出效果**

`fadeout(100)`

**淡入淡出切换**

`fadeToggle（1000）`

**渐进方式调至指定不透明度**

`fadeTo`

1. 效果；opacity，0-1之间
2. speed：三种预定速度之一的字符串，（1000）
3. easing：用来吧切换指定效果 



**自定义动画**

`animate`

语法：`animate （params，[speed],[easing],[fn]）`

参数
params：想要更改的样式属性，以对象的行驶本传递，必须写，属性名可以不带引号如果是复合则

**animate动画函数**
`$('body').animate({scrTop()})`

### **获取属性值**

**获取元素固有的属性值**

固有属性是指：a里的href input 里的type 
**prop（）**

**元素的自定义属性 attr**

**(div).attr（‘data-index’）**

### 事件委托

多个事件委托
`on`

```js
$('div').on{

  click:function(){
        
     },
    mouseenter:function(){
}

```

**同时绑定多个事件**

```js
$('div).on('click mouseenter'function(){
})
```

**事件委派

```js

$('ul).on('click,li',function(){
})

```



***on可以给未来元素创建事件***

**解绑事件**

`$('div).off('click)`

**解除事件委托**

`$('div).off('click‘ ’li’ )`

**触发一次事件**

```js
$('p').one('click'finction(){
alert()
})
```

**自动触发事件**

第一种简写模式

`$('div').click('click')`

第二种自动触发模式

```js
（'p’）.on（‘click’，function(){
alert('hi')
}）
$（‘p’）.trigger（‘click’）
```

**第三种**

`$('div').triggerHandler('click')`

**阻止事件默认行为** 

``event.preventDefault（）或者return false`

**阻止冒泡**

`event.stoppropagation`

#### **数据缓存**

`（div）.data（‘uname’，‘andy’）`  

**储存数据的格式**

`let todolist =  [{title:xxx,age:xxx]]`

### 文本内容

`html（）`查钊文本内容

`html（11）`更改

`text（）`

**获取表单的左中的val ()**

`$（input）.val("123")`

**截取字符串**

`substr`

**保留几位小数写法**

`(结果).to Fixed（2）`



### 遍历数组

第一个对象是索引号第二个对象是dom对象

`$（'div）.each(function(i,item){})`

i是索引  item是元素

**可遍历数组对象**

可遍历数组，对象 i是索引ele是内容
在对象情况下i是属性名ele是属性值

```js
$each()
$.each(object,funtion(i,ele){
})
```



### 添加dom元素

**内部添加**

把内容放到元素最前面面

`div.append('内容')`

把内容放到元素最后面

`div.prepend（li）`

**外部添加**

前面

`$('.test').after`

后面

`$('.test').before(div)`

**删除**

`$('div').remove` ,可以删除匹配元素
`$('div').empty()`可以删除匹配元素里的子节点
`$('div').html('')` 清空文本

### jquery尺寸与位置

1. width（）/height（）取得元素宽高只算width/height

2. innerWidth/innerHeight 取得匹配元素的宽高包含padding

3. outerWidt/outerHeight（）取得匹配元素的宽度和高度值 包含 padding border

4. outerWidth（true）/ outerheight（true） 取得元素匹配宽高，包含padding，border，margin

   

**位置**

获得相对于文档的值

`offset（）`

获得相对于父元素定位的值

`position（）`

可以获取被卷去的宽高一般是页面

`scrollTop（）/scrollLeft`

