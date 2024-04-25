# Vue

## 第一天



###  "requireConfigFile": false

```js
 "parserOptions": {

   "parser": "@babel/eslint-parser",

   "requireConfigFile": false

  },
```

**vue的基础概念**

**vue是什么，vue是一个渐进式的javascript框架**

![image-20221209233551665](image/\image-20221209233551665.png)

### 框架

![image-20221209233824822](image/\image-20221209233824822.png)



### vue是一个mvvm框架

![image-20221209234536027](image/\image-20221209234536027.png)



### vue组件化思想

![image-20221209235036013](image/\image-20221209235036013.png)

![image-20221209235146086](image/\image-20221209235146086.png)



###vue的两种开发模式

![image-20221209235611569](image/\image-20221209235611569.png)



### vue脚手架

![image-20221209235838715](image/\image-20221209235838715.png)

![image-20221209235903246](image/\image-20221209235903246.png)

### vue脚手架的基本使用

![image-20221210001919122](image/\image-20221210001919122.png)



### 如何覆盖脚手架下的webpack配置

![image-20221210010009610](image/\image-20221210010009610.png)

![image-20221210100105154](image/\image-20221210100105154.png)



### **vue组件**

![image-20221210101915393](image/\image-20221210101915393.png)

### vue中如何提供数据

![image-20221210114716771](image/\image-20221210114716771.png)

![image-20221210114743431](image/\image-20221210114743431.png)





### 插值表达式{{}}



代码

```vue

  <div>
    <div>{{ str }}</div>
    <div>
      {{ obj.name }}{{ obj.age > 18 ? "成年了" : "未成年" }}{{ obj.desc }}
    </div>
    <div></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      str: "陶正吃屎",
      obj: {
        name: "陶正",
        age: 16,
        desc: "性格妩媚",
      },
    };
  },
};
```



### vue指令

**vue指令：特殊的html标签属性，特点：v-开头**



**v-bind指令**

**作用：动态的设置html标签属性**

```js
  <div v-bind:title="taozheng">陶正吃屎</div>
</template>

<script>
export default {
  data() {
    return {
      taozheng: "陶正吃屎",
    };
  },
};
</script>
```

![image-20221210141754629](image/\image-20221210141754629.png)

**`-v-on`**

**作用：注册事件**

**语法：v-on：事件名='要执行的少量代码'**

![image-20221210154236342](image/\image-20221210154236342.png)



### 点击事件简写

![image-20221210154340585](image/\image-20221210154340585.png)





### vue中获取事件对象

![image-20221210160114154](image/\image-20221210160114154.png)

![image-20221210161006273](image/\image-20221210161006273.png)



### vue中的事件修饰符

![image-20221210162821493](image/\image-20221210162821493.png)

```vue

<a href="https://www.jd.com/" @click.prevent="fn1">去京东</a><br />
    <a href="https://world.taobao.com/?" @click.prevent="fn2('秘书')">去淘宝</a>
```



### 按键修饰符

![image-20221210165509480](image/\image-20221210165509480.png)

![image-20221210171451483](image/\image-20221210171451483.png)





## 第二天

### v-show和v-if

> **1.`v-show`**
>
> `语法：v-show="布尔值"（true显示，false隐藏）`
>
> `原理：实质是在控制元素的css样式，“display：none`
>
> 2.**`v-if`**
>
> `语法：v-if="布尔值"（true显示，false隐藏）`
>
> `原理：实质是在动态创建或删除元素节点`



### **v-else-if和v-else**



**注意：if/else /else if 需要连着写**

代码：

```js
 <h3 v-if="flag">尊敬的超级vip</h3>
    <h3 v-else>陶正啊？吃屎去吧</h3>
    <hr />
    <h3 v-if="age >= 30">30岁还房贷</h3>
    <h3 v-else-if="age >= 20">走去酒吧蹦迪</h3>
    <h3 v-else-if="age >= 10">好好学习吧小朋友</h3>
  </body>
</template>

<script>
export default {
  data() {
    return {
      flag: true,
      age: 12,
    };
  },
};
</script>
```





### v-model双向数据绑定

![image-20221210202226985](image/\image-20221210202226985.png)

### v-model的修饰符

![image-20221210211418254](image/\image-20221210211418254.png)



### v-text和v-html

![image-20221210213418307](image/\image-20221210213418307.png)

### v-for

**作用：可以遍历数组或者对象，用于渲染结构**

![image-20221210232116333](image/\image-20221210232116333.png)

**就地复用**

![image-20221213094832120](image/\image-20221213094832120.png)

### 虚拟dom

![image-20221213095758913](image/\image-20221213095758913.png)





### diff算法

![image-20221213100559556](image/\image-20221213100559556.png)

![image-20221213100923207](image/\image-20221213100923207.png)

### key的作用

![image-20221213110042383](image/\image-20221213110042383.png)

![image-20221213110228883](image/\image-20221213110228883.png)



### 计算属性

![image-20221213165402718](image/\image-20221213165402718.png)



![image-20221213170007267](image/\image-20221213170007267.png)

![image-20221213170451356](image/\image-20221213170451356.png)





















## 第三天

### 计算属性

![image-20221214115029312](image/\image-20221214115029312.png)



### 全选反选案例

```vue
<template>
  <div>
    <span>全选:</span>
    <input type="checkbox" v-model="isAll" />
    <button>反选</button>
    <ul>
      <li v-for="item in arr" :key="item.name">
        <input type="checkbox" v-model="item.c" />
        <span>{{ item.name }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {
          name: "猪八戒",
          c: false,
        },
        {
          name: "孙悟空",
          c: false,
        },
        {
          name: "唐僧",
          c: false,
        },
        {
          name: "白龙马",
          c: false,
        },
      ],
    };
  },
  computed: {
    isAll: {
      get() {
        return this.arr.every((item) => item.c === true);
      },
      set(value) {
        return this.arr.forEach((element) => {
          element.c = value;
        });
      },
    },
  },
};
```









### 侦听器

![image-20221214150353240](image/\image-20221214150353240.png)



监视简单类型：

```vue
<template>
  <body>
    <h3>银行卡余额：{{ money }}</h3>
    <button @click="money = money + 200">赚钱</button>
    <button @click="money = money - 20">吃饭</button>
  </body>
</template>

<script>
export default {
  data() {
    return {
      money: 200,
    };
  },
  watch: {
    money(newval, old) {
      alert("原来余额是" + old + "现在余额是" + newval);
    },
  },
};
</script>

<style>
</style>
```







监视复杂类型深度监视

```js
  watch: {
    money(newval, old) {
      alert("原来余额是" + old + "现在余额是" + newval);
    },
    obj: {
      deep: true,
      handler(newval) {
        alert(newval.i);
      },
    },
  },

```



**immediate**

值为true则立即执行，是否一进入页面就执行一次

![image-20221214160501700](image/\image-20221214160501700.png)





### 组件基础

封装组件的目的

![image-20221214170809589](image/\image-20221214170809589.png)

### 组件使用

![image-20221214171018006](image/\image-20221214171018006.png)



导入组件代码：

```vue
<template>
  <body>
    <HmHead></HmHead>
    <HmBody></HmBody>
    <HmFoot></HmFoot>
  </body>
</template>

<script>
import HmHead from "./commp/hm-head.vue";
import HmFoot from "./commp/hm-foot.vue";
import HmBody from "./commp/hm-body.vue";
export default {
  components: {
    HmFoot,
    HmHead,
    HmBody,
  },
};
</script>


<style>
</style>
```



### 组件注册

![image-20221214194215230](image/\image-20221214194215230.png)

![image-20221214200114688](image/\image-20221214200114688.png)



### 解决组件样式冲突

**vue默认组件样式是全局的，可以给style组件加上scoped**

![image-20221214201736109](image/\image-20221214201736109.png)



### 组件通信

![image-20221214203712860](image/\image-20221214203712860.png)

通过标签传值代码 （父传子）

```vue
<template>
  <div>
    <my-product title="鸡腿" price="12"></my-product>
    <my-product title="西瓜" price="22"></my-product>
    <my-product title="披萨" price="58"></my-product>
  </div>
</template>

<script>
import myProduct from "./compomemts/my-product.vue";

export default {
  components: { myProduct },
};
</script>

<style>
</style>
```

0



通过属性接收 （使用`props`以数组形式接收）

```vue
<template>
  <div class="product">
    <h3>标题：超级好吃的{{ title }}</h3>
    <p>价格：{{ price }}</p>
    <div>开业大酬宾</div>
  </div>
</template>

<script>
export default {
  name: "MyProduct",
  props: ["title", "price"],
};
</script>

<style lang="less" scoped>
.product {
  width: 400px;
  border: 3px solid black;
  border-radius: 5px;
  margin: 10px;
  padding: 0 20px;
}
</style>
```



### 单向数据流

![image-20221214215429007](image/\image-20221214215429007.png)

### 子传父

![image-20221214215645957](image/\image-20221214215645957.png)







## 第四天

### props

![image-20221215153935256](image/\image-20221215153935256.png)

![image-20221215161103486](image/\image-20221215161103486.png)

自定义校验

![image-20221215161855911](image/\image-20221215161855911.png)







## 第五天

### v-model是一个语法题

![image-20221216124139388](image/\image-20221216124139388.png)

![image-20221216164941214](image/\image-20221216164941214.png)







### 获取原生dom

`**利用ref和$refs可以获取dom元素或组件实例**`

![image-20221216165532220](image/\image-20221216165532220.png)





vue更新DOM是异步执行，所以不能直接同步获取，如果需要获取则可以

![image-20221216173322352](image/\image-20221216173322352.png)







### 动态组件

**dynamic动态组件**

![image-20221216200333061](image/\image-20221216200333061.png)

```vue
<template>
  <div id="app">
    <h1>动态组件dynamic的学习</h1>
    <button class="my-btn-primary" @click="comName = 'UserAccount'">
      账号密码填写
    </button>
    <button class="my-btn-success" @click="comName = 'userInfo'">
      个人信息填写
    </button>

    <!--
      动态组件的使用 ( component 组件 + is 属性 )
      (1) 设置挂载点<component>, (在哪显示)
      (2) 使用is属性来设置要显示哪个组件 (显示哪一个组件)
    -->
    <component :is="comName"></component>
  </div>
</template>

<script>
import UserAccount from "./components/UserAccount.vue";
import userInfo from "./components/UserInfo.vue";
export default {
  components: {
    UserAccount,
    userInfo,
  },
  data() {
    return {
      comName: "UserAccount",
    };
  },
};
</script>

<style lang="less">
#app {
  padding: 20px;
  .my-btn-primary {
    margin-right: 10px;
  }
}
</style>

```





### 自定义指令

局部注册语法 

```vue
<template>
  <body>
    <input type="text" v-focus />
  </body>
</template>

<script>
export default {
  //局部注册语法
  directives: {
    focus: {
        //钩子
      inserted(el) {                                
        el.focus();
      },
    },
  },
};
</script>

<style>
</style>
```



### 全局自定义指令

![image-20221216205433756](image/\image-20221216205433756.png)

代码：

```js
//Vue.directive(指令名，配置对象)
Vue.directive('border', {
  //钩子，指令所在元素被插入到页面节点中触发，此时可以操作dom
  inserted(el) {
    el.style.border = '10px red solid'
  }
})
```



### 自定义指令的值

代码：

app中data去给一个值

```vue
<body>
    <input type="text" v-focus />
    <div v-color="color1">陶正吃大便</div>
    <div v-color="color2">陶正吃屎</div>
  </body>
</template>

<script>
export default {
  data() {
    return {
      color1: "skyblue",
      color2: "orange",
    };
  },
```



**main.js中binding接收这个值**

```js
Vue.directive('color', {
  inserted(el, binding) {
    el.style.color = binding.value
  }，
     //值被修改时触发
  update(el, binding) {
    el.style.color = binding.value

  }
})
```



> **注意：inserted只会在元素被插入到页面中触发，指令的值修改时，不会被触发**
>
> **`update`**：**当指令值被修改时候会触发**







### 插槽-默认插槽

**插槽的基本语法**

![image-20221216214451015](image/\image-20221216214451015.png)



### 插槽默认值（后备内容）

![image-20221216220142778](image/\image-20221216220142778.png)



### 具名插槽

![](image/\image-20221216221607429.png)



**具名插槽定义**

```vue
<template>
  <body>
    <div class="dia">
      <div class="dialog">
        <!-- 插槽1 -->
        <slot name="head">我是后背内容</slot>
      </div>
      <div class="main">
        <!-- 插槽12-->

        <slot>我是后背内容</slot>
      </div>
      <div class="footer">
        <!-- 插槽3 -->

        <slot name="footer">我是后背内容</slot>
      </div>
    </div>
  </body>
</template>
```

**具名插槽分配**

```vue
<template>
  <body>
    <chacao>
      <!-- 具名插槽分配 -->

      <template v-slot:head>
        <h2>友情提示</h2>
      </template>
      <!-- 具名插槽分配 -->
      <template v-slot:main>
        用户名：<input type="text" /><br />
        密码：<input type="password" />
      </template>
      <!-- 具名插槽分配 -->

      <template v-slot:footer>
        <button>登录</button>
        <button>关闭</button>
      </template>
    </chacao>
  </body>
</template>
```



> **`注意v-slot可以简化成#`**



### 作用域插槽

![image-20221216224053692](image/\image-20221216224053692.png)





















## 第六天



###组件的生命周期

![image-20221218161932382](image/\image-20221218161932382.png)

### 生命周期-钩子函数概述

![image-20221218164853033](image/\image-20221218164853033.png)



**生命周期**

![](C:\Users\admin\Pictures\1\lifecycle.png)



### 路由基础

![image-20221218182452737](image/\image-20221218182452737.png)



### vue-router

![image-20221218202155108](image/\image-20221218202155108.png)



### vue-router的基本使用

![image-20221218205209370](image/\image-20221218205209370.png)



### 创建路由对象

![image-20221218210754842](image/\image-20221218210754842.png)

代码：

```js
import Find from './views/find.vue'
import My from './views/my.vue'
import Part from './views/part.vue'


Vue.config.productionTip = false
设置全局中间件
Vue.use(VueRouter)
创建实例
const router = new VueRouter({
   // 路由规则表
  routes: [
     // 路由规则
    { path: '/find', component: Find },
    { path: '/my', component: My },
    { path: '/part', component: Part }
  ]
})
new Vue({
  render: h => h(App),
  //挂载router
  router
}).$mount('#app')

```



 **指定路由出口**

```vue
      <router-view> </router-view>

```





### router-link

![image-20221218214607858](image/\image-20221218214607858.png)



### router-link的两个类名区别

![image-20221218220803879](image/\image-20221218220803879.png)











## 第七天

### 声明导航跳转传参

![image-20221219162721584](image/\image-20221219162721584.png)



### 路由重定向

```js
    { path: '/', redirect: '/find' }
```





### 动态路由

```js
        { path: '/my/:id', name: 'my', component: My },

```



**需要用params接收**

```js
    <p>{{ $route.params.id }}</p>

```



### vue-修改路由在地址栏的模式

![image-20221219171708145](image/\image-20221219171708145.png)



### 编程式导航-基本跳转

![image-20221219182050755](image/\image-20221219182050755.png)

### 编程式导航-路由传参

![image-20221219183355045](image/\image-20221219183355045.png)

![image-20221219185818376](image/\image-20221219185818376.png)





### keep-alive

![image-20221220002721148](image/\image-20221220002721148.png)

> **注：一旦组件被keep-alive包裹，从包裹那刻开始，组件又多了两个钩子**

![image-20221220003452276](image/\image-20221220003452276.png)





### 项目初始化

### 调整初始化目录结构

> 强烈建议大家严格按照老师的步骤进行调整，为了符合企业规范

为了更好的实现后面的操作，我们把整体的目录结构做一些调整。

目标:

1. 删除初始化的一些默认文件
2. 修改没删除的文件
3. 新增我们需要的目录结构

#### 修改文件

`public/index.html` 修改视口，开发的是移动端项目

```html
<meta
  name="viewport"
  content="width=device-width,initial-scale=1.0, user-scalable=0"
/>
```

`main.js` 不需要修改

`router/index.js`

删除默认的路由配置

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const routes = [
]

const router = new VueRouter({
  routes
})

export default router

```

`App.vue`

```html
<template>
  <div id="app">
    <router-view/>
  </div>
</template>
```

#### 删除文件

- src/views/AboutView.vue

- src/views/HomeView.vue

- src/components/HelloWorld.vue

- src/assets/logo.png

#### 新增目录

- src/api 目录
  - 存储接口模块 (发送ajax请求接口的模块)
- src/utils 目录
  - 存储一些工具模块 (自己封装的方法)

+ src/assets目录
  + 新增项目使用的素材

目录效果如下:

![image-20220613033022857](E:\vue基础笔记\01-PDF&笔记\images\image-20220613033022857.png)





## 第八天

### 封装的axios

```js
// axios模块
/* 封装axios用于发送请求 */
import axios from 'axios'

// 创建一个新的axios实例
const request = axios.create({
  baseURL: 'http://interview-api-t.itheima.net/h5/',
  timeout: 5000
})

// 添加请求拦截器
request.interceptors.request.use(function (config) {
  // 在发送请求之前做些什么
  return config
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error)
})

// 添加响应拦截器
request.interceptors.response.use(function (response) {
  // 对响应数据做点什么
  return response.data
}, function (error) {
  // 对响应错误做点什么
  return Promise.reject(error)
})

export default request

```



### 封装的token

```js
// 此处将会对token的操作进行封装，避免页面中，因为写错键名导致项目出错
export const setToken = (newToken) => {
  localStorage.setItem('hm-vant-mobile-token', newToken)
}

export const getToken = () => {
  return localStorage.getItem('hm-vant-mobile-token')
}

export const delToken = () => {
  localStorage.removeItem('hm-vant-mobile-token')
}

```



### 路由懒加载 & 异步组件

路由懒加载 & 异步组件， 不会一上来就将所有的组件都加载，而是访问到对应的路由了，才加载解析这个路由对应的所有组件

官网链接：https://router.vuejs.org/zh/guide/advanced/lazy-loading.html#%E4%BD%BF%E7%94%A8-webpack

> 当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

```js
const Detail = () => import('@/views/detail')
const Register = () => import('@/views/register')
const Login = () => import('@/views/login')
const Article = () => import('@/views/article')
const Collect = () => import('@/views/collect')
const Like = () => import('@/views/like')
const User = () => import('@/views/user')
```











## 第十天

### vuex基本概念

[中文文档](https://vuex.vuejs.org/zh/guide/)

vuex是vue的状态管理工具，**状态即数据**。 状态管理就是集中管理vue中 **通用的** 一些数据

注意（官方原文）：

- 不是所有的场景都适用于vuex，只有在必要的时候才使用vuex
- 使用了vuex之后，会附加更多的框架中的概念进来，增加了项目的复杂度  （数据的操作更便捷，数据的流动更清晰）

Vuex就像《近视眼镜》, 你自然会知道什么时候需要用它~



### vuex的优点: 方便的解决多组件的共享状态

 vuex的作用是解决《多组件状态共享》的问题。

- 它是独立于组件而单独存在的，所有的组件都可以把它当作  **一座桥梁** 来进行通讯。

- 特点：

  - **响应式**： 只要仓库一变化，其他所有地方都更新 （太爽了！！！）
  - 操作更简洁

  代码量非常少, 但是需要熟悉

![image-20201029061043482](E:\vue基础笔记\01-PDF&笔记\images\image-20201029061043482.png)



### 什么数据适合存到vuex中

一般情况下，只有  **多个组件均需要共享的数据** ，才有必要存储在vuex中，

对于某个组件中的私有数据，依旧存储在组件自身的data中。

例如：

- 对于所有组件而言，当前登陆的   **用户信息**  是需要在全体组件之间共享的，则它可以放在vuex中
- 对于文章详情页组件来说，当前的用户浏览的文章列表数据则应该属于这个组件的私有数据，应该要放在这个组件data中。



### 概述小结:

1. vuex解决什么问题?   vuex 能解决  **多组件共享数据**  的问题,  非常方便便捷
2. 什么样的数据, 适合存放到vuex?   多组件的  **通用**  的共用数据, 适合存到 vuex

vuex 两大优势:

1. 响应式变化
2. 操作简洁  (vuex提供了一些简化语法的辅助函数, 这些辅助函数, 需要熟练掌握)

### vuex 的使用 - 创建仓库

1 安装 vuex, 与vue-router类似，vuex是一个独立存在的插件，如果脚手架初始化没有选 vuex，就需要额外安装。

```
yarn add vuex@3.4.0
```

2 新建 `store/index.js` 专门存放 vuex

​	为了维护项目目录的整洁，在src目录下新建一个store目录其下放置一个index.js文件。 (和 `router/index.js` 类似)

​	![image-20201029064100611](E:\vue基础笔记\01-PDF&笔记\images\image-20201029064100611.png)

3 创建仓库 `store/index.js` 

```jsx
// 导入 vue
import Vue from 'vue'
// 导入 vuex
import Vuex from 'vuex'
// vuex也是vue的插件, 需要use一下, 进行插件的安装初始化
Vue.use(Vuex)

// 创建仓库 store
const store = new Vuex.Store()

// 导出仓库
export default store
```

4 在 main.js 中导入挂载到 Vue 实例上

```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  store
}).$mount('#app')
```

此刻起, 就成功创建了一个 **空仓库!!**



### 核心概念 - state 状态

State提供唯一的公共数据源，所有共享的数据都要统一放到Store中的State中存储。

打开项目中的store.js文件，在state对象中可以添加我们要共享的数据。

```jsx
// 创建仓库 store
const store = new Vuex.Store({
  // state 状态, 即数据, 类似于vue组件中的data,
  // 区别在于 data 是组件自己的数据, 而 state 中的数据整个vue项目的组件都能访问到
  state: {
    count: 101
  }
})
```

问题: 如何在组件中获取count?

1. 插值表达式 =》  {{  $store.state.count  }}
2. mapState 映射计算属性 =》  {{ count  }}



**1 原始形式- 插值表达式**

**`App.vue`**

组件中可以使用  **this.$store** 获取到vuex中的store对象实例，可通过**state**属性属性获取**count**， 如下

```vue
<h1>state的数据 - {{ $store.state.count }}</h1>
```

**计算属性** - 将state属性定义在计算属性中 https://vuex.vuejs.org/zh/guide/state.html

```js
// 把state中数据，定义在组件内的计算属性中
  computed: {
    count () {
      return this.$store.state.count
    }
  }
```

```vue
<h1>state的数据 - {{ count }}</h1>
```

但是每次, 都这样一个个的提供计算属性, 太麻烦了, 所以我们需要辅助函数 mapState 帮我们简化语法



**2 辅助函数  - mapState**

>mapState是辅助函数，帮助我们把store中的数据映射到 组件的计算属性中, 它属于一种方便的用法

用法 ： 

第一步：导入mapState (mapState是vuex中的一个函数)

```js
import { mapState } from 'vuex'
```

第二步：采用数组形式引入state属性

```js
mapState(['count']) 
```

> 上面代码的最终得到的是 **类似于**

```js
count () {
    return this.$store.state.count
}
```

第三步：利用**展开运算符**将导出的状态映射给计算属性

```js
  computed: {
    ...mapState(['count'])
  }
```

```vue
 <div> state的数据：{{ count }}</div>
```



### 核心概念 - mutations

### 基本使用

通过 `strict: true` 可以开启严格模式

> **state数据的修改只能通过mutations，并且mutations必须是同步的**

**定义mutations**

```js
const store  = new Vuex.Store({
  state: {
    count: 0
  },
  // 定义mutations
  mutations: {
     
  }
})
```

**格式说明**

mutations是一个对象，对象中存放修改state的方法

```js
mutations: {
    // 方法里参数 第一个参数是当前store的state属性
    // payload 载荷 运输参数 调用mutaiions的时候 可以传递参数 传递载荷
    addCount (state) {
      state.count += 1
    }
  },
```

组件中提交 mutations

```jsx
this.$store.commit('addCount')
```

**解决问题: 两个子组件, 添加操作 add,  addN 实现**



### 带参数的 mutation

需求: 父组件也希望能改到数据

提交 mutation 是可以传递参数的  `this.$store.commit('xxx',  参数)`

1 提供mutation函数

```js
mutations: {
  ...
  inputCount (state, count) {
    state.count = count
  }
},
```

2 注册事件

```jsx
<input type="text" :value="count" @input="handleInput">
```

3 提交mutation

```jsx
handleInput (e) {
  this.$store.commit('inputCount', +e.target.value)
}
```

**小tips: 提交的参数只能是一个, 如果有多个参数要传, 可以传递一个对象**

```jsx
this.$store.commit('inputCount', {
  count: e.target.value
})
```

**解决问题:  addN 的实现**



### **辅助函数** - mapMutations

> mapMutations和mapState很像，它把位于mutations中的方法提取了出来，我们可以将它导入

```js
import  { mapMutations } from 'vuex'
methods: {
    ...mapMutations(['addCount'])
}
```

> 上面代码的含义是将mutations的方法导入了methods中，等价于

```js
methods: {
      // commit(方法名, 载荷参数)
      addCount () {
          this.$store.commit('addCount')
      }
 }
```

此时，就可以直接通过this.addCount调用了

```jsx
<button @click="addCount">值+1</button>
```

但是请注意： Vuex中mutations中要求不能写异步代码，如果有异步的ajax请求，应该放置在actions中

### 核心概念-actions

> ### state是存放数据的，mutations是同步更新数据 (便于监测数据的变化, 更新视图等, 方便于调试工具查看变化)，
>
> actions则负责进行异步操作

**需求: 一秒钟之后, 要给一个数 去修改state**

![image-20201029074727223](E:\vue基础笔记\01-PDF&笔记\images\image-20201029074727223.png)

**定义actions**

```js
actions: {
  setAsyncCount (context, num) {
    // 一秒后, 给一个数, 去修改 num
    setTimeout(() => {
      context.commit('inputCount', num)
    }, 1000)
  }
},
```

**原始调用** - $store (支持传参)

```js
setAsyncCount () {
  this.$store.dispatch('setAsyncCount', 200)
}
```



**辅助函数** -mapActions

> actions也有辅助函数，可以将action导入到组件中

```js
import { mapActions } from 'vuex'
methods: {
    ...mapActions(['setAsyncCount'])
}
```

直接通过 this.方法 就可以调用

```vue
<button @click="setAsyncCount(200)">+异步</button>
```





### 核心概念-getters

> 除了state之外，有时我们还需要从state中派生出一些状态，这些状态是依赖state的，此时会用到getters

例如，state中定义了list，为1-10的数组，

```js
state: {
    list: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
}
```

组件中，需要显示所有大于5的数据，正常的方式，是需要list在组件中进行再一步的处理，但是getters可以帮助我们实现它

**定义getters**

```js
  getters: {
    // getters函数的第一个参数是 state
    // 必须要有返回值
     filterList:  state =>  state.list.filter(item => item > 5)
  }
```

使用getters

**原始方式** -$store

```vue
<div>{{ $store.getters.filterList }}</div>
```

**辅助函数** - mapGetters

```js
computed: {
    ...mapGetters(['filterList'])
}
```

```vue
 <div>{{ filterList }}</div>
```



### 核心概念 - 模块 module (**进阶拓展**)

> ### **由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。**

这句话的意思是，如果把所有的状态都放在state中，当项目变得越来越大的时候，Vuex会变得越来越难以维护

由此，又有了Vuex的模块化

![image-20201029155607101](E:\vue基础笔记\01-PDF&笔记\images\image-20201029155607101.png)



### **模块定义** - 准备 state

定义两个模块   **user** 和  **setting**

user中管理用户的信息状态  userInfo  `modules/user.js`

```jsx
const state = {
  userInfo: {
    name: 'zs',
    age: 18
  }
}

const mutations = {}

const actions = {}

const getters = {}

export default {
  state,
  mutations,
  actions,
  getters
}

```

setting中管理项目应用的名称 title, desc  `modules/setting.js`

```jsx
const state = {
  title: '这是大标题',
  desc: '描述真呀真不错'
}

const mutations = {}

const actions = {}

const getters = {}

export default {
  state,
  mutations,
  actions,
  getters
}
```

使用模块中的数据,  可以直接通过模块名访问 `$store.state.模块名.xxx`  =>  `$store.state.setting.title`

也可以通过 mapState 映射



### 命名空间 namespaced

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的

这句话的意思是 刚才的 user模块 还是 setting模块，它的 action、mutation 和 getter 其实并没有区分，都可以直接通过全局的方式调用, 如下图所示:

![image-20201029163627229](images\image-20201029163627229.png)

但是，如果我们想保证内部模块的高封闭性，我们可以采用namespaced来进行设置

`modules/user.js`

```jsx
const state = {
  userInfo: {
    name: 'zs',
    age: 18
  },
  myMsg: '我的数据'
}

const mutations = {
  updateMsg (state, msg) {
    state.myMsg = msg
  }
}

const actions = {}

const getters = {}

export default {
  namespaced: true,
  state,
  mutations,
  actions,
  getters
}
```

提交模块中的mutation

```jsx
全局的:   this.$store.commit('mutation函数名', 参数)

模块中的: this.$store.commit('模块名/mutation函数名', 参数)
```

namespaced: true 后, 要添加映射, 可以加上模块名, 找对应模块的 state/mutations/actions/getters

```jsx
computed: {
  // 全局的
  ...mapState(['count']),
  // 模块中的
  ...mapState('user', ['myMsg']),
},
methods: {
  // 全局的
  ...mapMutations(['addCount'])
  // 模块中的
  ...mapMutations('user', ['updateMsg'])
}
```



### 语法总结

![image-20230117144947836](image/\image-20230117144947836.png)
