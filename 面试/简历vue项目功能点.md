# Vue简历功能点

## 1. 登录模块

- token存储位置? 为什么要存在这些地方?
- 长token怎么用? 存储时间?
- 短token怎么用? 存储时间?

## 2.登录权限模块

- 通过什么技术实现登录权限监听? 从permission.js里面代码开始说
- 登录权限的业务逻辑是什么?

## 3.反向代理

- 你们公司项目开发环境反向代理是怎么配置的? 生产环境要配置反向代理吗?
- 你们公司项目请求的基本路径是如何做到开发环境和生产环境配置切换的?

## 4.请求拦截器

- 请求拦截器做了什么?
- 响应拦截器做了什么?

## 5.全局公共组件

- 封装过哪些公共组件?怎么注册到全局的?

- 有没有对element组件做过二次封装?(图片上传组件就是二次封装)

  - [x] 首先要说一下组件封装遵循什么? 

    答: 我们需要注意的主要是五部分：props、$emit、slot、多层嵌套组件数据传递、样式

  - props：表示组件接收的参数，最好用对象的写法，这样可以针对每个属性设置类型、默认值或自定义校验属性的值，此外还可以通过type、validator等方式对输入进行验证
  - $emit：子组件向父组件传递消息的重要途径  
  - slot：可以给组件动态插入一些内容或组件，是实现高阶组件的重要途径；当需要多个插槽时，可以使用具名slot
  - 如果涉及到多级组件嵌套传递数据时, 还需要用到$attrs, $linsteners, 以及依赖注入功能 
  - 样式做好隔离, 使用scoped


  - [x] 接下来说一下图片上传组件封装过程

    1. 对el-upload图片上传组件进行二次封装, 修改了一些样式使它符合公司的要求

    2. 组件内使用props定义了文件上传个数, 判断组件内存储图片的变量长度是否和父组件传递过来的个数一致, 控制上传图片按钮的显示和隐藏. 

    3. 组件内调用文件上传之前触发的钩子函数before-upload, 对文件类型, 文件大小进行判断, 如果不符合项目要求, return false, 阻止上传.

    4. 组件内通过:http-request自定义文件上传事件,  新增一个props属性,  需要父组件传递一个函数过来. 在自定义上传的事件中调用这个父组件传递过来的方法. 这样组件的图片上传动作交给组件的调用者控制
     - [x] 这里可能会问你, 如何判断文件类型和文件大小   
     - [ ] 答: before-upload钩子函数接受一个file形参, file.size就是文件大小, file.type就是文件类型

    5. 组件内封装了进度条功能, 使用的是element的el-progress组件. 当然, 这个进度条是假的, 只需要在上传文件前开启进度条,进度为0. 上传成功之后进度为100, 然后关闭进度条即可
     - [x] 这里可能会问你: 如何实现真实的文件上传进度数据? 
     - [ ] 答: 使用axios发送请求时, 配置项有一个提供好的事件, onUploadProgress(e) {}                                             可以通过e参数中的(e.loaded / e.total)换算出上传进度的百分比, 但是这个有兼容性要求, 需要XHR-level2支持的浏览器才行

    6. 组件内提供了点击查看大图的功能, 用户点击预览按钮时, el-upload组件会触发一个事件, 配合el-dialog弹层组件, 把文件地址给弹层中的img标签的src

    7. 组件提供了图片预览功能, 只需要父组件向子组件传递url地址即可, 组件内通过一个变量存储父组件通过props传递过来的url地址, 展示图片预览


- 工作日历组件二次封装


- excel导入组件封装, 通过onScuess事件拿到导入的excel文件内容
  1. 可以展开说一说拿到文件内容后, 对象的key值是中文的,后端要求字段是英文的,自己怎么做的
  2. 可以展开说一说拿到文件内容后, excel表格内的日期格式是从1900年开始计算的, 自己怎么处理日期的

- excel导出功能实现
  1. 定义导出表格的头部属性是什么? 是什么数据格式?
  2. 定义导出表格的文件名字属性是什么?
  3. 定义导出表格的内容属性是什么? 是什么数据格式?
  4. 可以说一下如何导出复杂表头的?
  5. 对导出功能的第三方插件Export2Excel.js, 如何做优化的?(按需导入)

## 6.自定义指令

1. 项目中有没有用过自定义指令?场景说一下(处理头像图片加载失败功能)?
2. 自定义指令语法?(说一下自定义指令钩子函数和形参)

## 7. 树形结构模块

1. 树形数据用的是什么组件?
2. 树形数据怎么生成? (说公司给的数据, 自己怎么写递归函数的, 大概思路)
3. 如何指定树形组件数据的父,子节点参数?.

## 8.路由权限模块

1. 项目中分了静态路由, 动态路由, 目的是什么?

   答: 静态路由是所有用户都可以访问的路由. 动态路由是根据后端返回的权限数据进行过滤后生成的有权限的路由

2. RBAC权限思路是什么?

   - 后端会在我们获取用户信息的时候返回一个数组, 里面包含了有权限的路由名称
   - 我们把路由分为静态路由和动态路由
   - 静态路由是所有人都可以访问的
   - 动态路由是根据后端返回的权限字段过滤后生成的
   - 在vuex创建一个permission模块, 用于存储渲染侧边栏的路由表
   - 模块中初始路由表只是我们定义好的静态路由
   - 拿到所有的动态路由数据和后端返回的路由权限进行过滤, 生成有权限的动态路由表
   - 合并静态路由表和动态路由表, 实现侧边栏渲染
   - 调用router.addRouters(), 给路由实例添加动态路由,实现有权限的路由访问

3. 在做路由权限的时候遇到的404页面问题, 描述下产生原因, 怎么解决的?

   - 因为404路由配置是存储在静态路由中的, 我们添加的动态路由在404路由的后面, 这样刷新页面的时候, 路由匹配规则或从路由表第一项开始查找, 还没查到动态添加的路由就被404路由拦截了
   - 解决方案: 删除静态路由表中的404路由配置, 手动添加到router.addRouters([...动态路由表, {404路由表}])方法内

4. 做路由权限时候, 刷新页面白屏问题, 如何解决的?

   - 先解决了404问题后, 再刷新页面就出白屏问题
   - 在router.addRouters()后面调用  next(to.path) 可解决

5. 做路由权限时候, 用户登出后,换一个账号登录,发现还可以访问之前用户的路由页面, 如何解决的?

   - 因为通过router.addRouters()添加的路由信息没被清空, 换账号后又调用router.addRouters()方法添加路由.
   - 解决: 在登出按钮事件触发的时候,调用重置路由的方法即可(重置路由的方法需要封装)

## 9.按钮权限模块

1. 项目中如何实现根据不同角色限制按钮使用的?
   - 按钮限制使用需要使用属性  :disabled="true/false" 实现
   - 获取用户信息的时候后端会返回按钮权限的字段
   - 因为不同的组件中都会有按钮要调用一个方法判断是禁用, 所以我们定义一个mixin.js文件, 定义一个vue的mixin混入功能
   - 混入功能定义一个方法, 根据传入的参数和在后端返回的权限字段中查找是否存在, 返回true或者false
   - 在相应的组件中注册混入对象, 在需要判断权限的按钮中调用混入对象中的方法, 返回true/false实现按钮的权限
2. mixin和vuex区别是什么?
   - vuex的数据是共享的, 任意一个组件修改vuex中的数据, 其他组件中也会跟着变
   - mixin中的数据在每一个组件中使用都是独立的
3. mixin中属性和vue组件中的属性冲突怎么办?
   - 优先使用vue组件中的
   - 生命周期函数冲突, 先执行混入对象里的, 再执行vue组件中的

## 10.Echarts图表

1. 如何在vue中使用echarts图标, 步骤?
2. 循环遍历的图表, 如何做自适应效果?
3. 你做过哪些图表?常见配置有哪些?(背)

## 11.element-ui国际化功能(慎说, 涉及到公司对外业务)

## 12. Vue项目优化(必问!!!)

1. ### Vue 代码层面的优化

   - ##### v-if 和 v-show 区分使用场景

     v-if 适用于在运行时很少改变条件，不需要频繁切换条件的场景；v-show 则适用于需要非常频繁切换条件的场景

   - ##### computed 和 watch  区分使用场景

     computed: 当我们需要进行数值计算，并且依赖于其它数据时，应该使用 computed，因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算

     watch:  当我们需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API )

   - ##### v-for 遍历必须为 item 添加 key. 且避免同时使用 v-if

     key值最好是唯一的值, 尽量不要用索引, 因为渲染表单是容易数据错乱

     v-for优先级比v-if高, 会先渲染页面再判断显示隐藏, 所以可以在v-for外面包裹一层template标签,里面写v-if

   - ##### 长列表性能优化

     ```js
        // 一次性获取上万条数据,页面很卡顿怎么做?
         // ①和后台沟通使用分页功能
         // ②把数据切割, 通过定时器, 每隔一定的时间往要渲染的变量里push切割后的数据
         async getEmployeeSimple() {
           const res = await getEmployeeSimple() // 1万条数据
           for (let i = 0; i < res.length; i += 20) {
             setTimeout(() => {
               // 每隔800毫秒渲染20条数据
               this.people.push(...res.slice(i, i + 20))
             }, i * 40)
           }
         }
     ```

   - ##### 图片资源懒加载

     对于图片过多的页面，为了加速页面加载速度，所以很多时候我们需要将页面内未出现在可视区域内的图片先不做加载， 等到滚动到可视区域后再去加载

     可以使用vue-lazyload 插件

     ①下载插件 npm install vue-lazyload

     ②导入插件  main.js 中 导入  import VueLazyload from 'vue-lazyload'

     ③注册插件  Vue.use(VueLazyload)

     ④使用插件  <img v-lazy="/static/img/1.png">  这样图片会在进入视口后加载图片

     图片懒加载原理:  

     1. img标签的src不放真正的地址(可以放一张空白图片占位)  

     2. img标签的自定义属性存放真正的地址  
     3. 图片一旦进入视口,就把自定义属性中的地址给src, 图片就加载出来了
     4. 算法:  if(图片距离文档顶部的距离 <= 页面滚动出去的距离 + 屏幕高度) { 说明图片进入了视口 }
     5. img.offsetTop <= window.pageYOffset + document.body.clientHeight

   - ##### 第三方插件的按需引入

     elementUI我们采用的是全局一次性导入, 最好的方式是在main.js中按需导入

2. ### webpack 配置层面的优化

   - ##### Webpack 对图片进行压缩

     在 vue 项目中除了可以在 vue.config.js 中 url-loader 中设置 limit 大小来对图片处理，对小于 limit 的图片转化为 base64 格式，其余的不做操作。所以对有些较大的图片资源，在请求资源的时候，加载会很慢，我们可以用 image-webpack-loader来压缩图片

     ①安装 image-webpack-loader    ----   npm install image-webpack-loader --save-dev

     ②在vue.config.js 中进行配置

     ```js
     {
       test: /.(png|jpe?g|gif|svg)(?.*)?$/,
       use:[
         {
         loader: 'url-loader',
         options: {
           limit: 10000,
           name: utils.assetsPath('img/[name].[hash:7].[ext]')
           }
         },
         {
           loader: 'image-webpack-loader',
           options: {
             bypassOnDebug: true,
           }
         }
       ]
     }
     ```

   - ##### 减少 ES6 转为 ES5 的冗余代码

     Babel 插件会在将 ES6 代码转换成 ES5 代码时会注入一些辅助函数

     ①安装 babel-plugin-transform-runtime  ----  npm install babel-plugin-transform-runtime --save-dev

     ②项目根目录找到 .babelrc  配置文件

     ```js
     "plugins": [
         "transform-runtime"
     ]
     ```

   - ##### 提取公共代码(如果你在项目中抽离了公共代码, 就不用配置这个了)

     如果项目中没有去将每个页面的第三方库和公共模块提取出来，则项目会存在以下问题：

     ①相同的资源被重复加载，浪费用户的流量和服务器的成本。

     ②每个页面需要加载的资源太大，导致网页首屏加载缓慢，影响用户体验

     不需要安装第三方包, webpack提供了这个功能

     ```js
     new webpack.optimize.CommonsChunkPlugin({
       name: 'vendor',
       minChunks: function(module, count) {
         return (
           module.resource &&
           /\.js$/.test(module.resource) &&
           module.resource.indexOf(
             path.join(__dirname, '../node_modules')
           ) === 0
         );
       }
     }),
     // 抽取出代码模块的映射关系
     new webpack.optimize.CommonsChunkPlugin({
       name: 'manifest',
       chunks: ['vendor']
     })
     ```

   3. ### 浏览器层面的优化

- ##### 开启 gzip 压缩

  gizp压缩是一种http请求优化方式，通过减少文件体积来提高加载速度。html、js、css文件甚至json数据都可以用它压缩，可以减小60%以上的体积

  两种方法: 1. 前端使用webpack配置   2.nginx服务器配置  (一般采用两种结合的方式)

  ①安装依赖   npm install --save-dev compression-webpack-plugin@1.1.12

  ②找到vue.config.js

  ```js
  const CompressionPlugin = require('compression-webpack-plugin');
  module.exports = {
          plugins: [
              new CompressionPlugin({
                  algorithm: 'gzip', // 使用gzip压缩
                  test: /\.js$|\.html$|\.css$/, // 匹配文件名
                  filename: '[path].gz[query]', // 压缩后的文件名(保持原文件名，后缀加.gz)
                  minRatio: 1, // 压缩率小于1才会压缩
                  threshold: 10240, // 对超过10k的数据压缩
                  deleteOriginalAssets: false, // 是否删除未压缩的源文件，谨慎设置，如果希望提供非gzip的资源，可不设置或者设置为false（比如删除打包后的gz后还可以加载到原始资源文件）
              }),
          ],
      },
  };
  ```

  ngnix配置:  配置完要重启nginx服务器  命令: nginx -s  reload

  ```js
  http {
      include       mime.types;
      default_type  application/octet-stream;
      sendfile        on;
      #tcp_nopush     on;
      #keepalive_timeout  0;
      keepalive_timeout  65;
      # 开启gzip
      gzip  on;
      gzip_static on; 
      # 设置缓冲区大小
      gzip_buffers 4 16k;
      #压缩级别官网建议是6
      gzip_comp_level 6;
      #压缩的类型
      gzip_types text/plain application/javascript text/css application/xml text/javascript application/x-httpd-php;
      server {
          listen       8462;
          server_name  localhost;
          location / {
              root   dist;
              index  index.html index.htm;
          }
          error_page   500 502 503 504  /50x.html;
          location = /50x.html {
              root   html;
          }
      }
  }
  ```

- #### 浏览器缓存(关键字:  强缓存和协商缓存)

  在Web性能优化上，我们总是追求网页能快速打开相应，这时就是浏览器缓存上场的时间了

  概念：当浏览器向服务器发起资源请求后，相应后会加载各种资源：HTML页面、图片、JS文件、CSS文件，对于一些不经常变的内容，浏览器会将它们缓存下来，等下次再访问的时候，就直接从客户端加载这些资源了。

  这些被浏览器保存下来的资源成为缓存。（注意：和cookie和localStorage是不同的概念）

  1. ##### 浏览器缓存机制

     浏览器是否缓存、缓存多久都是由服务器来决定的

      准确的说，当浏览器发起资源请求后，在服务器返回的响应头中的某些字段就包含了有关缓存的信息

  2. ##### 强缓存：

     http状态码为200(可能是强缓存, 也可能是从服务器拉的数据)

     ajax请求的数据成功都是200, 但是这个不是缓存

  3. ##### 协商缓存:

     http状态码为304 (发一次请求到服务器, 问服务器资源是否更改过, 如果服务器告诉个客户端没有更改, 那么客户端会从自身拿数据, 就是304)

     当浏览器对某个资源的请求没有命中强缓存，就会发一个请求到服务器，验证协商缓存是否命中，如果协商缓存命中，请求响应返回的http状态为304

  4. **选择合适的缓存策略**(后端要设置的)

     - 对于某些不需要缓存的资源，可以使用 `Cache-control: no-store` ，表示该资源不需要缓存

     - 对于频繁变动的资源，可以使用 `Cache-Control: no-cache` 并配合 `ETag` 使用，表示该资源已被缓存，但是每次都会发送请求询问资源是否更新

     - 对于代码文件来说，通常使用 `Cache-Control: max-age=31536000` (秒为单位)  并配合策略缓存使用，然后对文件进行指纹处理，一旦文件名变动就会立刻下载新的文件

     - ```js
       //nginx配置
       #静态资源文件缓存  static目录下都设置缓存
         location ^~ /static/ {
           access_log off;
           add_header Cache-Control max-age=2592000;
           expires  30d;
         }
       ```

  5. ##### 浏览器缓存的优点(重点)

     a、加快页面加载的速度，提升性能

     b、缓解服务端的压力（减少请求次数）

     c、减少带宽消耗

  6. ##### 浏览器缺点

     有时候一些实时数据更新的页面是不能使用浏览器缓存的（如股票类网站实时更新数据），会出现一些错误的数据。

  7. ##### 如果面试官问你, 如何请求服务端设置了缓存的文件

     ①html页面中设置  <meta HTTP-EQUIV="pargma" CONTENT="no-catch">

     ②发送的请求中: url地址后面拼接随机数或者时间戳  

     ```js
     axios({
         url: 'www.baidu.com?ran=' + Math.random() 
         url: 'www.baidu.com?timeStamp=' + +new Date() 
     })
     ```

  8. ##### 利用Vue脚手架提供的工具  npm run preview -- --report

     ①分析哪些第三方js插件占用空间比较大

     ②在vue.config.js中排除他们, 不进行打包

     ```js
     // 排除 elementUI xlsx  和 vue 
       externals:
           {
             'vue': 'Vue',
             'element-ui': 'ELEMENT',
             'xlsx': 'XLSX',
             'echarts': 'echarts'
          }
     ```

     ③在vue.config.js中使用CND托管

     ```js
     const cdn = {
         css: [
           'https://unpkg.com/element-ui/lib/theme-chalk/index.css' // 提前引入elementUI样式
         ], // 放置css文件目录
         js: [ // cdn托管的地址
           'https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js', // vuejs
           'https://unpkg.com/element-ui@2.13.2/lib/index.js', // element
           'https://cdn.jsdelivr.net/npm/xlsx@0.16.9/dist/xlsx.full.min.js', // xlsx 相关
           'https://cdn.jsdelivr.net/npm/xlsx@0.16.9/dist/jszip.min.js', // xlsx 相关
           'https://cdn.jsdelivr.net/npm/echarts@5.4.0/dist/echarts.min.js' // echarts 相关
         ] // 放置js文件目录
       }
     // 之后通过 html-webpack-plugin中配置
     // 配置cdn
         config.plugin('html').tap(args => {
           args[0].cdn = cdn
           return args
         })
     ```

     ④手动在public文件夹中的html引入cdn托管的文件

     ```html
     <!-- 引入JS -->
     <% for(var js of htmlWebpackPlugin.options.cdn.js) { %>
       <script src="<%=js%>"></script>
     <% } %>
     <!-- 引入样式 -->
       <% for(var css of htmlWebpackPlugin.options.cdn.css) { %>
         <link rel="stylesheet" href="<%=css%>">
       <% } %>   
     ```


## 



## 15. 目前比较流行的加密方式RSA(非对称性加密)

- 核心思路: 后端给一个公钥(用于加密数据), 我们拿到这个公钥后,去加密数据,然后发送给后端, 后端去用私钥解密(跟咱们无关)

- 上代码

  ```javascript
  //1. 前端安装jsencrypt包   npm i jsencrypt -D
  //2. 在登录页,导入这个包  import JSEncrypt from 'jsencrypt'
  //3. 找到登录按钮方法,里面写如下代码
  //3.1 调用后端获取公钥的接口,拿到公钥
  const publicKey = await getPublicKey()   //这个方法自行去api中定义
  //3.2 创建本地的加密实例
  let encryptor = new JSEncrypt() //这个构造函数是导入的方法
  //3.3 解密公钥
  encryptor.setPublicKey(publicKey)
  //3.4 加密我们的密码
  this.password = encryptor.encrypt(this.password)  //data中定义的双向绑定获取用户输入的密码
  //3.5 发送登录请求的接口, 传入账户和密码, 登录成功存token, 跳转页面
  ```

## 16. element-ui的自定义主题设置

- 思路: 官方网站下载自定义主题包, 放到项目的public目录下, 点击切换主题按钮, 切换document文档的link标签的href属性的主题包css地址即可
  1. 去官网下载主题包   https://element.eleme.cn/2.13/?#/zh-CN/theme
  2. 把下载的主题包存到项目的public文件夹下的theme文件夹内
  3. 删除main.js中引入的element-ui的css
  4. 找到public文件夹下的index.html, 手动使用link标签导入一个默认主题,加个Id属性,方便将来操作这个link标签
  5. 随便写个切换按钮,注册点击事件,根据点击按钮不同, 根据Id获取到link标签,修改他href地址
  6. 注意: 这个地址必须看项目打包后的文件地址, 因为public文件下的文件不会被webpack打包

## 17. 网站与主题同步换肤(非elementUI的样式也想切换)

思路

1. 使用的是CSS3的变量语法, 提前给需要更换的非主题样式使用CSS变量

   ```css
   html {//声明CSS3的全局变量, 2个
       --mainTextColor: red;
       --mainBgColor: orange;
   }
   ```

2. 默认情况下给非主题标签使用定义好的全局变量

   ```css
   //例子, 给某个class类名设置默认样式
   nav {
       color: var(--mainTextColor); //文字颜色红色
       backgroundColor: var(--mainBgColor); //背景色橙色
   }
   .box {
       color: var(--mainTextColor); //文字颜色红色
       backgroundColor: var(--mainBgColor); //背景色橙色
   }
   ```

3. 用户点击切换主题按钮, 我们通过js动态的修改html变量的值

   ```js
   document.documentElement.style.setProperty('--mainTextColor', 'black')
   document.documentElement.style.setProperty('--mainBgColor', 'blue')
   ```


## 18 JWT和TOKEN区别

1. 前端使用方式, 两种无任何差别, 都是登录成功, 拿到返回的字符串 ,存储起来,发送请求时候随请求头一起发送给后端
2. 他们的区别只是服务器验证方式的区别
3. 服务端验证客户端发来的token信息要进行数据的查询操作；而JWT验证客户端发来的token信息不需要， JWT使用密钥校验不用数据库的查询

