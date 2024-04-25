# javascript高级

[TOC]





## 第一天

### **作用域链**

在函数被执行是会优先查找当前作用域链的变量
如果作业域链查不到则会逐级查找父级作用域直到全局作用域
不在用到的内存并且没有被回收的就叫内存泄漏
全局变量一般不会被回收
一般情况下局部变量 的值不用了就会被 回收

**内存的生命周期**

内存分配，内存使用，内存回收
浏览器垃圾 回收算法：引用计数法 和标记清除法

### **闭包**

就是让外部可以调用函数内部的值
怎么理解闭包
***闭包 = 内存函数+外层函数的变量***
**闭包的作用**
封闭数据实现数据私有外部页可以访问函数内部的变量
闭包有用因为他允许将函数与其所操作的数据环境关联

*起来闭包可能引起的 问题**内存泄漏***

***闭包的格式***



```html

<script>
    //语法：
function fn (){
         let a = 0
         function fb(){
        a++
        console.log(a);
         }
         return fb
     }

let fc = fn()
</script>
```

------



### 数组参数

**动态参数**

argunments
arguments是函数内部内置的伪数组变量他包含了调用函数时候传入的所有实参 



**剩余参数**

function（a，b...a）
剩余参数可以设置实参
剩余参数得到的是真数组 



**展开运算符**

可以展开数组配合Math可以求最大值最小值

求最大值 `Math.max（...arr）`

求最小值 `Math.min（...arr）`

排序： 

```html
<script>
let arr =[a,b,c] = [1,2,3,4,5,6]
;[c3,b2,a1] = [c,b,a]
</script>
```

------



### 箭头函数

**基本语法**

```html
<script>
const fn = () =>{
console.log(123)
}
fn()
```



 

```html
<script>
let fn = x =>{
     console.log(x+x);
     }
fn(2)
当实参只有一个值且只有一行时候可以省略括号
 let fn = x =>console.log(x+x);   
fn(2)
```

------



### 解构

**基本语法**

```js
const arr [1,2,3]
const [max,min,avg]=arr
```

**简写**

```js
let arr =[a,b,c] = [1,2,3,4,5,6]
```

**交换变量**

```js
[c,b,a]=[a,b,c]
```

数组解构之前必须加分号

**多维数组解构**

语法

```js
const [a,b,[c,d]]=[1,2,[3,4]
```

**解构对象**

```js
const {uname,age}={uname:'xx',age:18}
```

**多级对象解构**

```html
<script>
let obj = {
    name:'陶正',
    pig:{
  mother:'pig妈妈',
  father:'pig爸爸'
    },
    age:18
}
const {name,pig:{ mother , father },age} = obj
```

------



### **forEach遍历数组方法**

> forEach没有返回值

基本语法：

```js
arr.forEach(function(item,index){

})
```

简化写法`arrt.forEach(*（item，i）* => {}`

------



###  filter方法

主要用于筛选符合条件的元素并返回筛选之后的新数组。filter有返回值

语法：

```js
const arr  =  [10,20,30,]
const newArr = arr.filter(function(item,index){
    return item >= 20
})
```

简写：

```js
  const newArr = arr.filter(item=>item>=20)
console.log(newArr);

```

------



### 案例

css补充写法

```css
 css点击谁谁变绿色 */
    .filter a:active,
    .filter a:focus {
      background: #05943c;
      color: #fff;
    }
```

案例精髓段落

一段判断大小返回数值的段落

```js
newArr = goodsList.filter(item=>item.price>0&&item.price<100)
```

大案例

```html
<script>
function rander(arr) {
      let str = ''

      arr.forEach(item => {
        const { name, price, picture } = item
        str += `
  <div class="item">
      <img src="${picture}" alt="">
      <p class="name">${name}</p>
      <p class="price">${price}</p>
    </div>
  
  `
      });

      document.querySelector('.list').innerHTML = str
    }
    rander(goodsList)
  
    document.querySelector('.filter').addEventListener('click', e => {
      const { tagName, dataset } = e.target
      if (tagName === 'A') {
        let arr = goodsList
        if (dataset.index === '1') {
      arr = goodsList.filter(item=>item.price>0&&item.price<100)
        }else if (dataset.index === '2') {
          arr = goodsList.filter(item => item.price >= 100 && item.price <= 300);
        } else if (dataset.index === '3') {
          arr = goodsList.filter(item => item.price >= 300)
        }
        rander(arr)
      }
    })
</script>
```



## 第二天

------



### 构造函数

1. 构造函数是一种函数，主要是用来初始化对象的。可以使用构造函数同时创建多个类似对象

2. 构造函数的通俗约定是函数首字母大写，例：Obj创建新的的类似对象要new Obj

3. 使用new关键字的函数行为被称为实例化

4. 实例化构造函数没有参数时候可以省略（）

5. 构造函数内部无需写return返回值即为新创建对象

6. 构造函数内部的return返回的值无效，所以不要写return

7. new Object（）new Date()也是实例化构造函数

8. 构造函数首字母是大写 

   

   **构造函数存在的问题**

   构造函数存在浪费内存的问题

   

   **构造函数基本语法**

   ```js
   function Obj(*name*, *age*,*id*){
   
     this.name = *name*
   
     this.age = *age*
   
     this.id = *id*
   
   }
   
   const obj = new Obj('冯俊',18,'666')
   ```

   

**实例化过程**

1. 创建新空对象

2. 构造函数this指向新对象

3. 执行构造函数代码，修改this添加新属性

4. 返回新对象

   

**实例成员和静态成员**

**实例成员**

1. 通过构造函数创建的对象称为实例对象

2. 实例对象的属性和方法即为实例成员

3. 构造函数创建的实例对象彼此互不影响

   

   **静态成员**

   1. 静态成员就是构造函数里的属性和方法
   2. 一般公共特征的属性或者方法静态成员设置为静态成员
   3. 静态成员方法中this指向构造函数本身

------



### 内置构造函数

- object

- Array

- string

- Number

  

#### **1.object**

获得对象中的属性值和属性名

`Object.keys(obj)`

获得对象中的属性值

`Object.values（）`

语法：

```js
const o = {name:'冯军',age:'18'}
const one = Object.keys(o);
const two = Object.values(o);
```

**对象拷贝**

经常给对象添加一个新的属性

`Object.assign(obj2,obj）`

`Object.assign(obj,{gender:'男})`

------



#### **2.Array**

数组常见-核心方法

> 1. `forEach` 作用：遍历数组，不返回，用于不改变值。查找打印输出值
> 2. `filter`作用： 过滤数组，筛选数组元素并生成新数组
> 3. `map` 作用迭代数组数组返回新数组，新数组元素是处理之后的值经常用于数据处理
> 4. `reduce` 作用累计器，返回累计处理的结果，经常用于求和
> 

#### set的概述

set这个方法是[es6](https://so.csdn.net/so/search?q=es6&spm=1001.2101.3001.7020)新提出来的，这个方法类似于数组，但是set里面的值是唯一的不允许重复。
set本身是一个[构造函数](https://so.csdn.net/so/search?q=构造函数&spm=1001.2101.3001.7020)，里面可以存储任意类型的数据的唯一值。

> 1. add()：用于数据的添加。
> 2. delete()：用于数据的删除。
> 3. size()：用于计算set对象的大小（相当于[数组](https://so.csdn.net/so/search?q=数组&spm=1001.2101.3001.7020)的长度）。
> 4. clear()：清空数据。
> 5. has()：用于寻找set对象中是否存在某个值。

**raduce方法**

作用：返回累计处理的结果，经常用于求和

基本语法：`arr.reduce(function(){},起始值)`

参数：起始值可以省略，如果作为第一次累计的起始值

代码案例：

```js
let arr = [1,2,3,4,5,6,7,8,9]
let num = arr.reduce(function(prev,item){
return prev+item
},0)

console.log(num);
```

简化写法：

```javascript

let arr = [1,2,3,4,5,6,7,8,9]
let num = arr.reduce((prev,item)=>prev+item)
console.log(num);
```

#### slice方法

**从已有的数组中返回选定的元素(数组单元的截取)**
用法：

**两个参数**
  `slice(参数1，参数2);`
参数1：从何处开始选取（截取数组单元起始位置的索引下标）
参数2：从何处结束选取（截取数组单元结束位置的索引下标）

```js
示例：

   var arr=['aa','bb','cc','dd','ee','ff'];
    var data=arr.slice(2,4);
    新数组data结果为： ["cc", "dd"]
```





**find方法**

实例方法`find`查找元素，返回符合测试条件的第一个元素值如果没有符合条件就返回undefined

基础语法

```js
let goods  = arr.find(function(item){
    return item.id  ===1
})
```

简化语法：

```js
let goods  = arr.find(item=>item.id===2)
```

**every方法**

实例方法`every`检测数组所有元素是否符合指定条件如果所有元素都通过检测则返回true，否则返回false

语法

```js
const flag = arr.every(item=>item>=1)
```

**转化数组**

伪数组转化为真数组

代码：

```js
 const div=document.querySelectorAll('div')
 console.log(div);
 const divs =Array.from(div)
console.log(divs);
```

排序：正序

![image-20221128145849990](image\image-20221128145849990.png)

倒序：

![image-20221128145919554](image\image-20221128145919554.png)

 数组常见方法-其他方法

> 1. 实例方法`join`数组拼接为字符串返回字符串
>
> 2. 实例方法`find`查找元素，返回符合测试条件的第一个元素值如果没有符合条件就返回undefined
>
> 3. `findIndex`（）查找第一索引i
>
> 4. 实例方法`every`检测数组所有元素是否符合指定条件如果所有元素都通过检测则返回true，否则返回false
>
> 5. 实例方法`some`检测数组中的元素 是否满足指定条件如果数组中有元素都满足条件则返回true否则返回false
>
> 6. 实例方法`concat`合并两个数组，返回新的数组
>
> 7. 实例方法`sort`对原数组单元值进行排序，各位数
>
> 8. 实例方法`splice`删除或替换原数组的单元
>
> 9. 实例方法`reverse`反转数组
>
> 10. 实例方法`findIndex`查找元素索引值
>
> 11. `includes` 判断某个字符是否包含在字符串里方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 `true`，否则返回 `false`。
>
>     



------



#### 3.String

##### **常见实例方法**

> 1. 实例属性`length`用来获取字符串的度长
> 2. #### 实例方法`split（‘分割符’）`用来将字符串拆分成数组
> 3. 实例方法`substring(需要截取的第一个索引[,结束的索引号])`用于字符串截取
> 4. 实例方法`startsWith（检测字符串[检测位置索引号]`）检测是否以某个字符开头
> 5. 实例方法`includes（搜索的字符串[,检测位置索引号]）`判断一个字符串是否包含在另一个字符串中根据情况返回true或者false
> 6. 实例方法`toUpperCase`用于将字母转换成大写
> 7. 实例方法`toLowerCase`用于将字母转换成小写
> 8. 实例方法`indexOf`检测是否包含某字符
> 9. 实例方法`endsWith`检测是否以某字符结尾
> 10. 实例方法`replace`用于替换字符串，支持正则匹配
> 11. 实例方法`match`用于查找字符串支持正则匹配
> 12. 实例方法`num.toString()`

------

**split字符串转数组方法**

```js
const str = 'pink,red,skybule'
const arr = str.split(',')
console.log(arr);
```

**substring字符串截取**

注：1.结束的索引号不包含想要截取的部分

​    2.如果省略索引号默认截取到最后

代码：

```js
const str  = '今天又要开始做核算了'
const str2 = str.substring(6,9)
console.log(str2);
```

**startsWith检测是否以某个字符开头**

不写数值默认是以第一个开头

写数值就是以从第一个开始数包括空格和符号，到第九个判断是否相符

代码：

```js
 const str = 'go to si,wode aisi'
        console.log((str.startsWith('go')));
        console.log((str.startsWith('wo',9)));
```





#### 4.Number

**toFixed方法**

1. 设置保留小数的长度
2. 保留两位小数四舍五入

基础语法：

`toFixed（1）`



### 大案例

```js
document.querySelector('.list').innerHTML = goodsList.map(item => {
    const { name, price, picture, count, spec, gift } = item
// 判断gift是否有值因为split不能去拆分空字符串
    const str = gift ? gift.split(',').map(item => `<span>【赠品】${item}</span></br>`).join('') : ''
    // 精度问题保留两位所有*100后/100
    const sbTotal = ((price * 100 * count) / 100).toFixed(2)

    return `
<div class="item">
      <img src="${picture}" alt="">
      <p class="name">${name} <span class="tag">${str}</span></p>
      <p class="spec">${Object.values(spec).join('/')}</p>
      <p class="price">${price.toFixed(2)}</p>
      <p class="count">x${count}</p>
      <p class="sub-total">${sbTotal}</p>
    </div>

`
  }).join('')
  const total = goodsList.reduce((prev, item) => prev + (item.price * 100 * item.count) / 100, 0).toFixed(2)
  document.querySelector('.amount').innerHTML = total
```



------







## 第三天

###编程思想

**1.面向过程**

**面向过程**就是分析除解决问题所需要的步骤，然后用函数把这些步骤一步步实现，使用时候在一个个调用。**分析步骤按照步骤解决问题**

**优点**：性能比面向对象高适合跟跟硬件关系很紧密的对象

**缺点**：没有面向对象易维护，易复用，易扩展

**2.面向对象**

**面向对象**就是把事务分解成为一个个对象，然后由对象之间分工合作。**面向对象是以功能性来划分问题，而不是步骤**

**面向对象特性：**

- 封装性
- 继承性
- 多态性

**优点**：易维护，易复用，易扩展，面向对象有封装,继承，多态性的特性，可以设计出低耦合的系统更加灵活，更加易于维护

**缺点**：性能比面向过程低

------



### 原型对象

- 构造函数通过原型分配的函数是所有对象共享的
- JavaScript规定，每一个构造函数都有一个**prototype**属性，指向另一个对象，所以我们也称为原型对象
- 这个对象可以挂载函数，对象实例化不会多次创建原型上函数节约内存
- 我们可以把那些不变的方法，直接定义在prototype对象上，这样所有对象的实例化就可以共享这些方法。
- 构造函数和原型对象中的this都指向实例化的对象

**原型对象**`prototype`

> 公共的属性写在构造函数里公共的方法写在原型对象上
>
> **构造函数和原型对象里的this指向实例对象**

代码：

```js
 // 构造函数  公共的属性和方法 封装到 Star 构造函数里面了
    // 1.公共的属性写到 构造函数里面
    let flag = null
    function Star(uname, age) {
      this.uname = uname
      this.age = age
      flag = this
    }
    // 2. 公共的方法写到原型对象身上   节约了内存
    Star.prototype.sing = function () {
      console.log('唱歌');
      flag=this
    }

    const ldh = new Star('刘德华', 55)
    const zxy = new Star('张学友', 58)
  
    console.log(ldh === flag);

```





**封装数组原型对像**

通过封装数组原型对象可以创建自定义方法

```js
// 封装一键求数组最大值
// 思路:自定义方法，将算法绑定在原型对象上
let arr = [2,3,4,5,6,100,100]
// 封装求最大值
Array.prototype.max=(function(){
    return Math.max(...this)

})
// 封装求最小值
    Array.prototype.min = (function () {
        return Math.min(...this)

    })
    // 封装求和
    Array.prototype.NumberAll = (function () {
        return this.reduce((prev,item)=>(prev+item),0)

    })

console.log(arr.max());
console.log(arr.min());
console.log(arr.NumberAll());
```



**constructor属性（构造函数）**



> 每个原型对象里都有个`constructor`（构造函数）属性
>
> 该属性指向该原型对象的构造函数



```js
function Star (){

}
console.log(Star.prototype.constructor===Star);
//constructor该属性指回了构造函数
```



作用:因为重新赋值会丢失构造函数，可以在重新赋值情况下找回构造函数

代码

```js
let arr = [1,2,3,4,5]
function Star (num){

}
    console.log(Star.prototype);
// constructor 可以重新指回构造函数
// 像这样设置对象就会丢失原型对象的构造函数
Star.prototype={
    // 解决就是用constructor重新获得构造函数
    constructor:Star,
    // 封装求最大值
    max : function () {
        return Math.max(...this)

    },
// 封装求最小值
   min: function () {
        return Math.min(...this)

    },
    // 封装求和
   NumberAll : (function () {
        return this.reduce((prev, item) => (prev + item), 0)

    })


}
console.log(prototype.max(arr));
console.log(Star.prototype);
```







### 对象原型`_proto_`

实例对象都有一个`_proto_`指向构造函数的prototype之所以我们对象可以使用构造函数prototype原型对象的属性和方法，就是因为对象有`_proto_`属性

![](image\image-20221120111759727.png)

Prototype就是实例对象里的对象原型

**注意**：

1. `_proto_`是非js标准
2. `[[prototype]]`和`_proto_`意义相同
3. 用来表明当前实例对象指向哪个原型对象`prototype`
4. `_proto_`对象原型里也有一个`constructor`属性指向创建该实例对象的构造函数
5. 该属性**只读**





> 1.**构造函数`new` 之后`this`指向了实例对象，同时创建了原型对象`prototype`**
>
> 2.**实例对象里的对象原型`_proto_`属性指向构造函数里的原型对象**
>
> 3.**同时`_proto_`对象原型里也有个`constructor`属性指向了创建该实例对象的构造函数``**
>
> 4.**同时`prototype`原型对象里的构造函数有个`constructor`属性又指向了构造函数，**
>
> 

![image-20221120120307771](image\image-20221120120307771.png)



**图例1：**

​                     实例对象的**对象原型**`指向`构造函数**原型对象**

![image-20221120115406613](image\image-20221120115406613.png)

```js
function Star (){

}
let newobj = new Star()
console.log(newobj.__proto__===Star.prototype);
```



**图例2：**

​                          **对象原型**的`constructor`属性`指回`了**构造函数**

![image-20221120115750451](image\image-20221120115750451.png)

```js
function Star (){

}
let newobj = new Star()
console.log(newobj.__proto__);
```



**图列3：**

​                              **总结**

![image-20221120121435117](image\image-20221120121435117.png)







------

### 原型继承

可以提取构造函数的公共部分

```js
// 原型继承
const Person = {
    name:'xiaoqi',
    age:18
}
function Woman(){}
//提取公共样式到prototype中
//woman继承preson
Woman.prototype = Person
//因为有重新赋值的关系所以要重新绑定constructor指向构造函数
Woman.prototype.constructor = Woman
//创建了一个新的实例化对象
const pink = new Woman()
console.log(pink);


function Man() { }
Man.prototype = Person
Man.prototype.constructor = Man
const red = new Man()
console.log(red);
```



**但是这样原型继承会存在问题就是他们的person指向是相同地址，当要给他们绑定某个单独的方法时会作用到全部上去**

图例：

![image-20221120133447555](image\image-20221120133447555.png)



**代码：**

```js
const Person = {
    name:'xiaoqi',
    age:18
}
function Woman(){}
//提取公共样式到prototype中
Woman.prototype = Person
//因为有重新赋值的关系所以要重新绑定constructor指向构造函数
Woman.prototype.constructor = Woman
// 当给woman赋值新方法时由于同一个地址指向导致man也出现了新的方法
Woman.prototype.baby = function(){
    console.log(baby);
}
const pink = new Woman()
console.log(pink);


function Man() { }
Man.prototype = Person
Man.prototype.constructor = Man
const red = new Man()
console.log(red);
```

**解决方法**

创建一个构造函数用这个**构造函数去new一个新对象**，因为**构造函数new的新对象是在内存空间开辟一个新空间来储存**，所以他们有**相同的值**，但是他们**指向的对象不同**，正因为如此所以给对**象赋值新值或方法时就不是指向同一个地址而是指向各自的地址**



**图例：**

![image-20221120135145342](image\image-20221120135145342.png)

**代码：**

```js
  // 构造函数  new 出来的对象 结构一样，但是对象不一样
    function Person() {
      this.eyes = 2
      this.head = 1
    }
    // console.log(new Person)
    // 女人  构造函数   继承  想要 继承 Person
    function Woman() {

    }
    // Woman 通过原型来继承 Person
    // 父构造函数（父类）   子构造函数（子类）
    // 子类的原型 =  new 父类  
    Woman.prototype = new Person()   // {eyes: 2, head: 1} 
    // 指回原来的构造函数
    Woman.prototype.constructor = Woman

    // 给女人添加一个方法  生孩子
    Woman.prototype.baby = function () {
      console.log('宝贝')
    }
    const red = new Woman()
    console.log(red)
    // console.log(Woman.prototype)
    // 男人 构造函数  继承  想要 继承 Person
    function Man() {

    }
    // 通过 原型继承 Person
    Man.prototype = new Person()
    Man.prototype.constructor = Man
    const pink = new Man()
    console.log(pink)
```



------



### 原型链

**只要是一个对象都有对象原型`__proto__`**

**只要是原型对象就会有`constructor`**指向构造函数

概述：

每一个对象里都会有一个原型对象`（proeotype）`，而每一个原型对象里都会有一个对象原型`__proto__`，而每一个**对象原型**`(__proto__)`都指向**父类原型对象`（proeotype）`**，所以**父类原型对象**`（proeotype）`里的**对象原型**`(__proto__)`指向谁？**原型对象**里的**对象原型**是指向，顶级对象`Object`**的**原型对象`（proeotype）`而**Object**的**原型对象**的**对象原型**又指向谁？事实上Object的对象原型已经没有指向所以返回`null`

**图例：**

![image-20221120143330384](image\image-20221120143330384.png)



**代码：**

```js
function Person (age,name){
    this.age = age
    this.name = name
}

const aq = new Person(18, '阿强')
// -------------------------------------------------------
/*实例对象的对象原型指向父类原型对象*/
console.log(aq.__proto__=== Person.prototype);
//--------------------------------------------------------
//父类对象的对象原型指向顶级对象的原型对象
console.log(Person.prototype.__proto__===Object.prototype);
//--------------------------------------------------------
//顶级对象里的对象原型对象的对象原型没有指向，所以返回一个null
console.log(Object.prototype.__proto__);

```



**概念图：**

![image-20221120143752873](image\image-20221120143752873.png)

**原型链的作用-查找规则**

1. 
1. 



------



### instanceof运算符

**作用**

> 可以检测对象是否在该原型链上



**图例：**

![image-20221120152853546](image\image-20221120152853546.png)



**代码:**

```js
function Person (age,name){
    this.age = age
    this.name = name
}
// 检测对象是否在原型链上
const aq = new Person(18, '阿强')
console.log(aq instanceof Person);
// ----------------------------------
console.log(aq instanceof Object);
// ----------------------------------
console.log(aq instanceof Array);

```



------



### 大案例

```js

// 大致思路：
// 把他们的公共部分例如弹出框封装到构造函数里，因为他们的this指向不同所以可以传不同的值进去
// 再把页面打开关闭挂载到原型上面这样就可以调用他们

    // 模态框的构造函数
function Modal (title,message){
  // 因为盒子是公共的所以一定要加this
this.title = title
this.message = message
//创建一个div将包裹公共部分
this.modaBox = document.createElement('div')
this.modaBox.className='modal'
this.modaBox.innerHTML=` 
<div class="header">${this.title} <i>x</i></div>
 <div class="body">${this.message}</div>`

}
//把关闭方法挂载到打开里面
// 封装方法
Modal.prototype.open=function(){
  // 判断有没有modal一个互斥锁
  if(!document.querySelector('.modal')){
      document.body.appendChild(this.modaBox)
      // 因为关闭按钮要在打开时候获取，同时this没有指向所以可以调用close关闭
    this.modaBox.querySelector('i').addEventListener('click', () => {
      this.close()
    })
  }

}
  Modal.prototype.close = function () {
    document.body.removeChild(this.modaBox)
  }


// 点击删除按钮

document.querySelector('#delete').addEventListener('click',()=>{
    //传值
const m = new Modal('提示','你没有权限删除')
m.open()
console.log(Modal.prototype);
})

  document.querySelector('#login').addEventListener('click', () => {
    const d = new Modal('温馨提示', '你还没有登录')
    d.open()
    console.log(Modal.prototype);
  })


```









------

##                       第四天



#### 深浅拷贝



##### `**浅拷贝**`



首先浅拷贝和深拷贝只针对引用类型

浅拷贝：拷贝的是地址

**常见方法**

拷贝对象：

`object.assgin（{...obj}）拷贝对象`

拷贝数组：

`Array.prototype.concat（）或者[...arr]`

**浅拷贝只适合拷贝简单数据类型，因为浅拷贝只拷贝栈内数据。如果是一层不影响如果出现多层对象则会有影响**





##### `**深拷贝**`

**常见方法**：

1. 通过递归实现深拷贝
2. lodash/cloneDeep
3. 通过JSON.stringify实现



**函数递归**

`如果一个函数可以在内部调用其本身，那么这个函数就是递归函数`

- 简单理解：函数内部自己调用自己，这个函数就是递归函数
- 递归函数的作用和循环效果类似
- 由于递归很容易发射栈溢出错误，所以必须要加退出条件return



**递归深拷贝**

例代码：

```js
const obj = {
    uname:'pink',
    age:18,
    arr:[`陈冠希`,'彭于晏','刘德华']
}
const o = {}

// 拷贝函数
function deepCopy(newObj,oldObj){

     
    for(let k in oldObj){
//判断当值是数组时，进行递归，递归后，值newObj是空数组，oldObj是数组
     if (oldObj[k] instanceof Array) {
         newObj[k] = []
         deepCopy(newObj[k], oldObj[k])
         //注意：要把数组写前面因为数组也是一个对象，否则会把数组当对象处理
         //判断当值是对象时，进行递归，递归后，值newObj是空对象，oldObj是对象
         if (oldObj[k] instanceof Object) {
             newObj[k] = {}
             deepCopy(newObj[k], oldObj[k])

      }else{
         newObj[k] = oldObj[k]
      }

        
    }
}
    deepCopy(o,obj)
    o.arr[0] = 'fj'
    console.log(obj);
    console.log(o);
```



**深拷贝的新对象不会影响旧对象**



**用lodash进行深拷贝**

代码

```js
 <!-- 先引用 -->
  <script src="./lodash.min.js"></script>
  <script>
    const obj = {
      uname: 'pink',
      age: 18,
      hobby: ['乒乓球', '足球'],
      family: {
        baby: '小pink'
      }
    }
   const o = _.cloneDeep(obj)
    console.log(o)
    o.family.baby = '老pink'
    console.log(obj)
  </script>
```





**利用JSON进行深拷贝**

```js 
   const obj = {
      uname: 'pink',
      age: 18,
      hobby: ['乒乓球', '足球'],
      family: {
        baby: '小pink'
      }
    }
    // 把对象转换为 JSON 字符串
    // console.log(JSON.stringify(obj))
  const o = JSON.parse(JSON.stringify(obj))
  o.age=22
  console.log(o);
  console.log(obj);
```



------



### **throw抛出异常**

**作用：**

异常处理是指预估代码执行中可能发生的错误，然后最大程度的避免代码无法继续运行 

抛出异常会中止程序

> 一般throw都配合`**new Error**方法`写 `throw new Error('参数不能为空')`

**图例：**

![image-20221120221432541](image\image-20221120221432541.png)

**代码**：

```js
function fn (one,two){
    if(!one||!two){
throw new Error('参数不能为空')
    }
    return one + two
}
console.log(fn());
```



**try/catch捕获错误信息**

1. 我们可以通过try/catch捕获错误信息（浏览器提供的信息）`try`试试，`catch`拦住，`finally`最后
2. 把可能发生错误的代码写到try里去
3.  拦截错误提供浏览器给出的错误提示，但是**不中断程序的执行**，可以加`return`或是`new Error`
4. `finally`代码一般和`try与catch`一起出现，`finally`不管上面代码**错与对**，**该行内代码一定会执行**，**但是该行之后代码就不再执行了**

![image-20221120223944386](image\image-20221120223944386.png)

**代码：**

```js
  function fn (){
        //把可能发生错误的代码写到try里去
     try{
         const div = document.querySelector('.div')
         div.style.color = 'red'

     }catch(err){
        //拦截错误提供浏览器给出的错误提示，但是不中断程序的执行，可以加return或是new Error
       console.log(err.message);
throw new Error('找不到div对象')
     }
    //  不管是否出现错误finally都会执行
    finally{
     console.log('必定执行');
    }

console.log(11);
    }
fn()
```



**debugger**断点

调试时可以直接跳转到`debugger`所在行，就是相当于打断点



------



### this指向

**普通函数**

普通函数的`this`指向谁调用指向谁

严格模式下普通函数this指向是**undefined**



**箭头函数**

箭头函数中的this指向与普通函数完全不同也完全不受调用方式的影响，**事实上箭头函数中不存在this**



1. 箭头函数会默认帮我们绑定外层的this值，所以在箭头函数中this的值和外层this的指向是一样的
2. 箭头函数中的this引用的就是就近作用域中的this
3. 向外层作用域中，一层层查找this直到找到为止

**箭头函数不适用**

构造函数，原型函数，字面量对象中函数（要用到this的地方），dom事件函数

**适用**

需要使用上层this的地方





#### 改变this指向

javascript中还允许指定函数中的this指向，有三个方法可以动态指向普通函数中的this指向

- `call()`
- `apply()`
- `bind()`



1.`call()`-了解

1. 使用call方法调用函数，同时指定被调用函数中的this值
2. 语法：`fun.call（thisArg，arg1，arg2）`
3. `thisArg`：在fun函数运行时指定this值
4. arg1，arg2：传递的其他参数
5. 返回值就是函数的返回值，因为它就是调用的函数





**图例：**

![image-20221120232120892](image\image-20221120232120892.png)

**代码：**

```js
const obj = {age:11}
function fn (x,y){
    console.log(this);// obj
    console.log(x+y); // 2
}
// 改变this指向，让this指向obj同时传参数
fn.call(obj,1,1)
```



`apply（）理解`

使用`apply`方法调用函数，同时指定被调用函数中的this值

语法：`fub.apply（thisArg，[argArray]）`

1. thisArg:在fun函数运行时指定的this
2. argsArray：传递的值，必须包含在`数组`里
3. 返回值就是函数返回值，因为他时调用函数
4. 因此apply主要跟数组有关系，比如使用Math.max()求数组最大值

**图例：**

![image-20221120234001613](image\image-20221120234001613.png)

**代码：**

```js
const obj = {age:11}
function fn (x,y,z,w){
    console.log(this);
    console.log(x+y+z+w);
}
// 改变this指向，让this指向obj同时传参数
fn.apply(obj,[1,2,3,4])
// 也可以用于求最大值最小值因为有返回值
let arr =[1,2,3,4,5,6,7,8,9,10]
const s = Math.max.apply(Math,arr)
console.log(s);
```





`bind()`**-重点**

**bind()方法不会调用函数，但是能改变函数内部this指向**

**语法：**`fun.bind（thisArg，arg1，arg2）`

- thisArg：在fun函数运行时候指向this
- arg1，arg2：传递其他参数
- 返回由指定的`this`值和初始化参数改造的 **原函数拷贝**(新函数)
- 当我们只想改变this指向，并且不再想要调用这个函数，可以使用bind，比如改变定时器内部的`this`指向
- 返回值是个函数但是这个函数里的this是更改过的

**图例：**

![image-20221121002044081](image\image-20221121002044081.png)

**代码：**

```js
const obj = {age:11}
function fn (){
    console.log(this);
    
}
// 用bind方法不调用函数但是会生成一个返回值这个返回值里的函数this是改变了的
const fun = fn.bind(obj)
fun() // 返回
fn()
```



**改变this指向小案例**

**代码：**

```js
<button>点击禁用</button>
    <script>
document.querySelector('button').addEventListener('click',function(){
this.disabled =true
setTimeout(function(){
    this.disabled = false
    alert('禁用解除')
}.bind(this),2000)

})

```



**总结**

`call apply bind`**总结**

- **相同点**

  

**都可以改变函数内部this指向**



- **区别点**

  1. call和apply会调用函数，并且改变函数内部this指向
  2. call和apply传递的参数不一样，call传递参数aru1，aru2形式apply必须数组形式
  3. bind不会调用函数，可以改变函数内部this指向

  

- **主要应用场景**

1. call调用函数并传递参数
2. apply经常跟数组有关系，比如借助数学对象实现数组最大值最小值
3. bind不调用函数，但是还想改变this指向，比如改变定时器内部的this指向





### **节流（throttle）**

**节流**就是指连续触发事件但是在n秒内只能执行一次函数

**开发场景**：轮播图点击效果，鼠标移动，页面缩放，滚动条滚动，都可以加节流

**代码：**

```js
    const box = document.querySelector('.box')
    let i = 1  // 让这个变量++
    // 鼠标移动函数
    function mouseMove() {
      box.innerHTML = ++i
      // 如果里面存在大量操作 dom 的情况，可能会卡顿
    }
    // console.log(mouseMove)
    // 节流函数 throttle 
    function throttle(fn, t) {
      // 起始时间
      let startTime = 0
      return function () {
        // 得到当前的时间
        let now = Date.now()
        // 判断如果大于等于 500 采取调用函数
        //判断现在的时间减去上一次时间是否大于500如果大于500证明过去了500函数可以执行
        // 如果没有就不执行实现节流效果
        if (now - startTime >= t) {
          // 调用函数
          fn()
          // 起始的时间 = 现在的时间   写在调用函数的下面 
          startTime = now
        }
      }
    }
    
    box.addEventListener('mousemove', throttle(mouseMove, 500))

  //  调用函数都有返回值而此处返回值相当于
  // function() {
  //     // 得到当前的时间
  //     let now = Date.now()
  //     // 判断如果大于等于 500 采取调用函数
  //     //判断现在的时间减去上一次时间是否大于500如果大于500证明过去了500函数可以执行
  //     // 如果没有就不执行实现节流效果
  //     if (now - startTime >= t) {
  //       // 调用函数
  //       fn()
  //       // 起始的时间 = 现在的时间   写在调用函数的下面 
  //       startTime = now
// 所以这个节流函数能执行
```





### 防抖（debpunce）

**防抖就是指触发事件后在n秒内函数只能执行一次，如果在n秒内又触发了事件则会重新计算函数执行时间**

例如：1-5在3触发了某事件，则要重新计算，而计算的就是3往后推5位数也就是8

防抖一般应用在**搜索框**

**代码：**

```js
   const box = document.querySelector('.box')
    let i = 1  // 让这个变量++
    // 鼠标移动函数
    function mouseMove() {
      box.innerHTML = ++i
      // 如果里面存在大量操作 dom 的情况，可能会卡顿
    }
    // 防抖函数
function debpunce (fn,t){
  let tiemId
  return function(){
      //定时器要两秒后才能执行如果
    // 滑动了定时器马上清零了，又开始重新计算
 if(tiemId) clearTimeout(tiemId)
 tiemId = setTimeout(()=>{
fn()
 },t)

  }
}

 box.addEventListener('mousemove',debpunce(mouseMove,200))

```





###  

------

### 视频事件

1. `ontimeupdate` 事件在音频视频（audio/video），当前位置改变时触发
2. onloadeddata 事件在当前帧的数据加载完成，而且还没有足够数据的下一帧时触发





### 案例

```js
    // 思路:设置视频事件在位置改变时触发，设置一个节流函数，每秒执行一次将数据保存到本地
const video = document.querySelector('video')
video.ontimeupdate= _.throttle(()=>{
  localStorage.setItem('times',video.currentTime)
},1000)
// 在页面打开时候就会触发获得本地存储内数据，并将其赋给视频
video.onloadeddata = ()=>{
console.log(11);
video.currentTime = localStorage.getItem('times') || 0
}

```



## 扩展

![image-20230108130543300](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108130543300.png)

![image-20230108131042468](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108131042468.png)

### Map

Map 数据集合
1、通过 new Map() 创建 Map 实例

```js
let map = new Map()
1
2、向 Map 集合中添加值-- key 可以是任意类型的数据

// 方式 1： 通过传入二维数组
let map1 = new Map([[1, 2], [3, 4], [5, 6]])

// 方式 2： 通过 set() 方法进行添加值
let map2 = new Map()
map2.set(.1, 2132)
map2.set(true, {})
map2.set({}, .31241)
map2.set(NaN, [1, 2, 3])
map2.set([], 32131312)
1
2
3
4
5
6
7
8
9
10
```

3、重复添加键，基础数据类型的键会将值覆盖，引用类型的不会,因为内存地址不一样

let map = new Map()
map.set(111, 222)
map.set(111, 3333)
map.set(true, [])
map.set(NaN, {})
map.set({}, 1111)
map.set({}, 333)
map.set([1,2], true)
map.set([1,2], 'rerffds')
1
2
3
4
5
6
7
8
9


4、Map 的方法：set()、get()、has()、delete()、clear()、size

let map = new Map([[true, 111], [{}, 4124], [NaN, false]])
let arr = [1, 2]
map.set(111, 222) // 设置 键 111，对应的值为 222
map.set(arr, {}) // 设置 键 为变量 arr，对应的值为 {}
map.get(111) // 获取键 111 ，对应的值  ---> 222
map.get(43141414) // 获取不存在的键 43141414 ，对应的值  ---> undefined
map.has(true) // 是否存在键 true ---> true
map.has({}) // 是否存在键 {} ---> false
map.has(arr) // 是否存在键 arr ---> false true
map.size // 5
map.clear() //清除所有成员数据
————————————————
版权声明：本文为CSDN博主「俺是老王」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_37600506/article/details/122719525



对象扩展方法



![image-20230108141353191](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108141353191.png)





### 多维数组转一维数组

### ![image-20230108141936545](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108141936545.png)



### 私有属性

![image-20230108142317801](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108142317801.png)



### promise.allsettled

### ![image-20230108145204742](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108145204742.png)



### String.prototyp.matchAll

![image-20230108145536207](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108145536207.png)



### 生成器函数基本使用

![image-20230108145716937](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108145716937.png)



![image-20230108150247859](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20230108150247859.png)
