# 综合 Vue、React、webpack、浏览器、node

# 综合
## 1. React和Vue的区别
(参考：https://juejin.im/post/6844904040564785159)

- 相同点：

   1、react和vue都支持服务端渲染
    
   2、利用虚拟DOM实现快速渲染，组件化开发，通过props传参进行父子组件数据的传递
    
   3、都是数据驱动视图
    
   4、都有支持native的方案（react的react native，vue的weex）
    
   5、都有状态管理（react有redux，vue有vuex）

- 不同点：

  1、监听数据变化的实现原理不同
    - Vue 通过 getter/setter 以及一些函数的劫持，能精确知道数据变化，不需要特别的优化就能达到很好的性能
    - React 默认是通过比较引用的方式进行的，如果不优化（PureComponent/shouldComponentUpdate）可能导致大量不必要的VDOM的重新渲染
    
  2、数据流的不同
    - vue实现了数据的双向绑定react数据流动是单向的

  3、HoC 和 mixins
    - 在 Vue 中我们组合不同功能的方式是通过 mixin，而在React中我们通过 HoC (高阶组件）。
  
  4、组件通信的区别
    - Vue中子组件向父组件传递消息有两种方式：事件和回调函数，而且Vue更倾向于使用事件。但是在 React 中我们都是使用回调函数的，这可能是他们二者最大的区别。
 
  5、模板渲染方式的不同
    - React 是通过JSX渲染模板。React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的
    - Vue是通过一种拓展的HTML语法进行渲染。Vue是在和组件JS代码 分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现
   
  6、Vuex 和 Redux 的区别
    - Redux 使用的是不可变数据，而Vuex的数据是可变的。Redux每次都是用新的state替换旧的state，而Vuex是直接修改。
    - Redux 在检测数据变化的时候，是通过 diff 的方式比较差异的，而Vuex其实和Vue的原理一样，是通过 getter/setter来比较的（如果看Vuex源码会知道，其实他内部直接创建一个Vue实例用来跟踪数据变化）。
### vue 优缺点
- 优点：
	1. API设计上简单，语法简单，学习成本低
	2. 构建方面不包含路由和ajax功能，使用vuex, vue-router
	3. 指令（dom）和组件（视图，数据，逻辑）处理清晰
	4. 性能好，容易优化
	5. 基于依赖追踪的观察系统，并且异步队列更新
	6. 更快的渲染速度和更小的体积

- 缺点：
	1. Vue 不缺入门教程，可是很缺乏高阶教程与文档。同样的还有书籍。
	2. VUE不支持IE8
	3. 生态环境差不如angular和react
	4. 社区不大

### React 优缺点
- 优点：
	1. React速度很快：它并不直接对DOM进行操作，引入了一个叫做虚拟DOM的概念，安插在javascript逻辑和实际的DOM之间，性能好。最大限度减少DOM交互。
	2. 跨浏览器兼容：虚拟DOM帮助我们解决了跨浏览器问题，它为我们提供了标准化的API，甚至在IE8中都是没问题的。
	3. 一切都是component：代码更加模块化，重用代码更容易，可维护性高。这样当某个或某些组件出现问题是，可以方便地进行隔离。每个组件都可以进行独立的开发和测试，并且它们可以引入其它组件。这等同于提高了代码的可维护性。
	4. 单向数据流：Flux是一个用于在JavaScript应用中创建单向数据层的架构，它随着React视图库的开发而被Facebook概念化。减少了重复代码，这也是它为什么比传统数据绑定更简单。
	5. 同构、纯粹的javascript：因为搜索引擎的爬虫程序依赖的是服务端响应而不是JavaScript的执行，预渲染你的应用有助于搜索引擎优化。
	6. 兼容性好：比如使用RequireJS来加载和打包，而Browserify和Webpack适用于构建大型应用。它们使得那些艰难的任务不再让人望而生畏

- 缺点：
	1. React 只是一个视图库，而不是一个完整的框架。
	2. 对于 Web 开发初学者来说，有一个学习曲线。
	3. 将 React 集成到传统的 MVC 框架中需要一些额外的配置。
	4. 代码复杂性随着内联模板和 JSX 的增加而增加。
	5. 如果有太多的小组件可能增加项目的庞大和复杂。

## 2. MVC、MVP和MVVM的区别
1. MVC
  - View：负责视图显示，将Model中的数据显示出来。检测用户的键盘、鼠标等行为，传递调用Controller执行应用逻辑,View更新需要重新获取Model的数据
  - Controller：负责View和Model之间协作的应用逻辑或业务逻辑处理，根据用户行为对Model中的数据进行修改。
  - Model：负责保存数据，Model变更后，通过观察者模式通知View更新视图。
2. MVP
  - Passive View：View不再处理同步逻辑，对Presenter提供接口调用。由于不再依赖Model，可以让View从特定的业务场景中抽离，完全可以做到组件化。
  - Presenter（Supervising Controller）： 和经典MVC的Controller相比，
  - 任务更加繁重，不仅要处理应用业务逻辑，还要处理同步逻辑(高层次复杂的UI操作)。
  - Model：Model变更后，通过观察者模式通知Presenter，如果有视图更新，Presenter又可能调用View的接口更新视图。
3. MVVM
  - ViewModel：内部集成了Binder(Data-binding Engine，数据绑定引擎)，在MVP中派发器View或Model的更新都需要通过Presenter手动设置，而Binder则会实现View和Model的双向绑定，从而实现View或Model的自动更新。
  - View：可组件化，例如目前各种流行的UI组件框架，View的变化会通过Binder自动更新相应的Model。
  - Model：Model的变化会被Binder监听(仍然是通过观察者模式)，一旦监听到变化，Binder就会自动实现视图的更新。

4. 区别
   - 三者都是框架模式，它们的设计目标都是为了解决Model和View的耦合问题。
   - MVC模式出现较早主要应用在后端，在前端领域早期也有应用，如Backbone.js。它的优点是职责分离，分层清晰，多视图更新，缺点是数据流混乱，灵活性带来的维护性问题，依赖强烈，View强依赖Model(特定业务场景)，因此View无法组件化设计。
   - MVP模式在是MVC的进化形式，View可组件化设计，Presenter作为中间层负责MV通信，解决了两者耦合问题，但P层过于臃肿会导致维护问题。
   - MVVM模式在前端领域广泛应用，它不仅解决MV耦合问题，还同时解决了维护两者映射关系的大量繁杂代码和DOM操作代码，在提高开发效果，可读性同时还保持了优越的性能表现。 
  

## 3. 怎么将前端项目部署到服务器
1. 购买云服务器
    - 注意：如果想对外宣传的话，需要购买域名
    - 操作方法：
    1. 购买域名（产品菜单->域名注册->购买）
    2. 解析（控制台->域名）
    3. 备案
2. 搭建服务器环境
    - 就是你本地开发那个环境，放在服务器上，不过设置成生产环境。

3. 部署项目
    - 建议用git，在本地安装好git后，找一个git服务器托管，git push，上传进去，再登录服务器用git clone下载下来，启动服务器，大功告成！

## 4. 前端工程化的认识

前端工程化可以分成四个方面来说，分别为模块化、组件化、规范化和自动化。

### 模块化
- 模块：将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 最后进行统一的打包和加载，这样能够很好的保证高效的多人协作。
- 模块化的好处：更好的分离, 按需加载；更高复用性；高可维护性。
- 模块化规范：
  - CommonJS：module.export/require，模块输出的是值的拷贝，模块是运行时加载
  - AMD：define()/require()，依赖前置、提前执行
  - CMD：define()/seajs.use()，依赖就近，延迟执行
  - ES6 Module：export/import，模块输出的是值的引用，模块是在编译时加载

### 组件化
不同于模块化，模块化是对文件、对代码和资源拆分，而组件化则是对 UI 层面的拆分。

### 规范化
指的是我们在工程开发初期以及开发期间制定的系列规范

### 自动化
从最早先的 grunt、gulp 等，再到目前的 webpack、parcel。这些自动化工具在自动化合并、构建、打包都能为我们节省很多工作。



## 5. 前端性能优化
### 前端性能优化测试工具
- Google PageSpeed inSights
- WebPagetest

### 前端性能指标
- 加载指标：秒开率
- 稳定性指标：资源错误，JS报错，内存堆栈，接口报错等
- 操作体验指标：响应延迟，卡顿，滚动流畅性，TTI(可交互时间)，FID(用户首次和页面交互到页面响应的时间)，动画流畅性
- 加载链路的优化：从访问url到页面呈现，整个加载渲染链路可以优化的思路。

### 性能优化
代码层面：避免使用css表达式，避免使用高级选择器，通配选择器。

缓存利用：缓存Ajax，使用CDN，使用外部js和css文件以便缓存，添加Expires头，服务端配置Etag，减少DNS查找等

请求数量：合并样式和脚本，使用css图片精灵，初始首屏之外的图片资源按需加载，静态资源延迟加载。

请求带宽：压缩文件，开启GZIP

### 性能优化具体方案
1. 把html,css,js,font,img这些资源放在cdn上
2. 尽可能少的加载外链的css和js代码，在头部增加dns-prefetch，减少DNS解析.`<link rel="dns-prefetch" href="//d.3.cn"/>`
3. 不是在首屏显示的资源，不要立即加载，使用懒加载，数据延迟分批加载
4. 减少http请求次数和请求资源的大小
  - 资源合并压缩
  - 字体图标 base64
  - 考虑资源合并请求
5. script加载脚本会阻塞浏览器主线程，考虑异步化
6. 利用好缓存：http响应头缓存字段开启静态资源缓存，减少资源下载
7. 解析渲染优化


### 雅虎35条军规（和上面可能有重复，参考：https://www.jianshu.com/p/fa25121c0f5f）
1. 页面内容：减少http请求，减少DNS查询，避免重定向，缓存AJAX请求，延迟加载，预加载，减少DOM数量，划分内容到不同域名，尽量减少iframe的使用，避免404错误。
2. 服务器：使用CDN，添加Expires和Cache-Control响应头，启用Gzip压缩，配置Etag，尽早输出(flush)缓冲，Ajax请求使用GET方法，避免图片src为空
3. Cookie：减少Cookie大小，静态资源使用无Cookie域名
4. CSS：把样式表放在<head>中，不要使用CSS表达式，使用<link>替代@import，不要使用filter
5. JavaSript：把脚本放在页面底部，使用外部JS和CSS，压缩JS和CSS，移除重复脚本，减少DOM操作，使用高效的事件处理
6. 图片：优化图片，优化CSS Sprite，不要在HTML中缩放图片，使用体积小可缓存的favicon.ico
7. 移动端：保证所有组件都小于25k，把组件打包到一个复合文档里。

### 提高用户体验
1. 根据雅虎35条优化建议保证页面的载入速度。
2. 做好的页面结构，页面重构和用户体验；
3. 对每一像素和细节的苛刻。好的用户体验应该从细节开始，并贯穿于每一个细节，能够让用户有所感知
4. 保证页面的浏览器兼容性，做到可用性和可访问性。

## 6. 前端常见兼容性问题
- 样式兼容
    1. 不同浏览器的默认样式存在差异，可以使用 Normalize.css 抹平这些差异。当然，你也可以定制属于自己业务的 reset.css。比如加一个全局的*{margin:0;padding:0;}来统一。
    2. 部分属性需加浏览器前缀
    ![](https://user-gold-cdn.xitu.io/2020/7/12/173411678b5f4d45?w=642&h=386&f=png&s=20955)

- 交互兼容性
    1. 添加绑定事件：
    	![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2809e1e331e7412389aa5a726384d577~tplv-k3u1fbpfcp-zoom-1.image)
    2. 取消事件监听
    	![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5898b060a509437384e8641bef3825ad~tplv-k3u1fbpfcp-zoom-1.image)
    3. 阻止事件冒泡
     	![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6a2ee32f76c144f180b6bbc3984756fb~tplv-k3u1fbpfcp-zoom-1.image)
    4. 取消默认事件
        // event.preventDefault();
        // event.returnValue = false; // IE9以下的浏览器
    5. 事件对象：`event = event || window.event;`
    6. new Date()构造函数使用，正确的兼容用法是'2018/07/05'
    7. 求窗口大小的兼容写法clientWidth、scrollWidth、offsetWidth、scrollTop： 
    例：document.documentElement.scrollTop||document.body.scrollTop;

## 7. 移动端适配方案

### 物理像素和逻辑像素
- 物理像素

设备像素 = 物理像素，设备像素就是屏幕中一个个的点,设备分辨率就是横向点的个数 * 纵向点的个数


- 逻辑像素

设备独立像素 = CSS 像素 = 逻辑像素

### 移动端适配方案
1. rem
  - 设置根元素的字体大小，1rem = 根元素字体大小。CSS用媒体查询根据不同宽度设置根元素字体大小，或者用JS  `document.documentElement.style.fontSize = 100 * (clientWidth /750) + 'px';` 其中100是设计稿的html大小，750是假设的设计稿大小。
  - 写入CSS的尺寸 = UI图标注的尺寸/100px*1rem, 100px为该设计稿对应的html的font-size
  - rem与em的区别：em相对父级元素设置的font-size来设置大小 如果父元素没有设置font-size ，则继续向上查找，直至有设置font-size元素

2. viewport
  - 使用了理想视口，固定高度，宽度自适应。
  `<meta name="viewport" content="width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">`
  - 固定布局视口宽度，使用viewport进行缩放。如荔枝FM的网页宽度始终为640px。缩放比例scale为：`var scale = window.screen.width / 640`
  `<meta name="viewport" content="width=640, minimum-scale = 0.5625, maximum-scale = 0.5625, target-densitydpi=device-dpi">`

3. vw/vh：vw表示相对于视口的宽度，vh表示相对于视口高度

### pc端适配(可以三种方式复合使用)
1. 宽度百分比(流式布局)
2. 媒体查询(响应式布局)，Bootstrap就是用的这种方式布局，用这种方式需要用Js去兼容ie低版本，动态创建css
2. flex(弹性布局)

## 8. 滚动吸顶实现方式
(参考：https://juejin.im/post/6844903815041269774)
### 实现方式（后三种方案都是在滚动监听事件里判断是否满足吸顶条件即滚动容器的已滚动高度scrollTop 是否大于需要吸顶的元素到已定位的父级元素或者距离页面顶部的距离offsetTop，满足条件就把吸顶元素设置为position: fixed）
1. 使用 position:sticky 实现
2. 使用 JQuery 的 offset().top 实现
3. 使用原生的 offsetTop 实现
4. 使用 obj.getBoundingClientRect().top 实现
  - 这个 API 可以告诉你页面中某个元素相对浏览器视窗上下左右的距离。使用 obj.getBoundingClientRect().top 代替 scrollTop - offsetTop

### 问题
1. 吸顶的那一刻伴随抖动
  - 出现抖动的原因：在吸顶元素 position 变为 fixed 的时候，该元素就脱离了文档流，下一个元素就进行了补位。就是这个补位操作造成了抖动。
  - 解决为这个吸顶元素添加一个等高的父元素，我们监听这个父元素的 getBoundingClientRect().top 值来实现吸顶效果
2. 吸顶效果不能及时响应
  - 描述：当页面往下滚动时，吸顶元素需要等页面滚动停止之后才会出现吸顶效果
当页面往上滚动时，滚动到吸顶元素恢复文档流位置时吸顶元素不恢复原样，而等页面停止滚动之后才会恢复原样
  - 原因：在 ios 系统上不能实时监听 scroll 滚动监听事件，在滚动停止时才触发其相关的事件。
  - 解决方案：IOS 使用 position:sticky，Android 使用滚动监听 getBoundingClientRect().top 的值。

## 9. 图片懒加载
1. 方案1：将资源路径赋值到img标签的data-xx属性中，而非直接赋值在src属性。监听滚动事件，
持续判断图片是否在用户当前窗口的可视范围内，从而动态将data-xx的值(url)赋值到src里去。判断条件是img的el.getBoundingClientRect().top 小于window.innerHeight（或者document.documentElement.clientHeight），则需要展示图片。可以通过节流函数
2. 方案2：通过IntersectionObserver api判断图片是否出现在可视区域内，不需要监听Scroll来判断。在回调函数中用获取到的目标元素的可见比例intersectionRatio大于0或者isIntersecting为true，则目标元素可见。IntersectionObserver是浏览器原生提供的构造函数，接受两个参数：callback是可见性变化时的回调函数，option是配置对象（该参数可选）

## 10.拖拽

### 我的小程序里的拖拽组件
需要拖拽的元素设置为绝对定位，设一个初始位置，在catchtouchmove事件里控制拖拽元素只能在规定范围内移动，控制条件是根据鼠标在窗口中的垂直水平坐标（e.touches[0].clientY和e.touches[0].clientX）是否超过窗口的高度宽度或者小于0，如果超过了则设置top或者left为范围内的最大值，如果小于0则设置为0。

### H5 拖拽功能
#### 1. 先设置需要拖拽的元素draggable属性为true，表示可拖拽。
#### 2. 事件
 - 拖拽元素支持的事件
   - ondrag 应用于拖拽元素，整个拖拽过程都会调用
   - ondragstart 应用于拖拽元素，当拖拽开始时调用
   - ondragleave 应用于拖拽元素，当鼠标离开拖拽元素是调用
   - ondragend 应用于拖拽元素，当拖拽结束时调用
 - 目标元素支持的事件
   - ondragenter 应用于目标元素，当拖拽元素进入时调用
   - ondragover 应用于目标元素，当停留在目标元素上时调用
   - ondrop 应用于目标元素，当在目标元素上松开鼠标时调用(拖拽元素在目标元素上释放时)
   - ondragleave 应用于目标元素，当鼠标离开目标元素时调用
#### 3. dataTransfer对象
- dataTransfer对象允许在其上存储数据，这使得在被拖动元素与目标元素之间传送信息成为可能。
- 属性：dropEffect、effectAllowed、files、types
- 事件：setData、getData、clearData、setDragImage
#### 4. 注意：
调用 preventDefault() 来避免浏览器对数据的默认处理（drop 事件的默认行为是以链接形式打开）

## 11. 移动端点击事件延迟300毫秒
### 原因：
300 毫秒延迟的主要原因是解决双击缩放(double tap to zoom)。当用户一次点击屏幕之后，浏览器并不能立刻判断用户是确实要打开这个链接，还是想要进行双击操作，因此浏览器就等待 300 毫秒，以判断用户是否再次点击了屏幕。

### 解决方案：
1. 添加viewpoint meta标签，禁止用户进行缩放。
`<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />`

2. FastClick

移动端事件触发顺序：在移动端，手指点击一个元素，会经过：touchstart --> touchmove -> touchend -->click。

fastclick.js的原理是：FastClick的实现原理是在检测到touchend事件的时候，会通过DOM自定义事件立即出发模拟一个click事件，并把浏览器在300ms之后真正的click事件阻止掉。

vue中使用

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8fa62185a8aa4bbbb3e4ef4f82a5c6c6~tplv-k3u1fbpfcp-zoom-1.image)

## 12. 点击穿透
### 发生原因：
 - 手指按下之后，会先执行touch事件，然后记录点击的坐标，300ms之后(移动端使用鼠标事件会有300毫秒的延迟)，在该坐标上查找元素，如果该元素绑定了鼠标事件，就把事件执行了
### 点透发生的条件：
 - A 和 B不是后代继承关系(如果是后代继承关系的话，就直接是冒泡子类的话题了)
 - A发生touch， A touch后立即消失， B事件绑定click
 - A z-index大于B，即A显示在B浮层之上
### 解决方案
1. `A.addEventListener('touchend', function(e) {e.preventDefault(); });`
2. 用定时器延迟3ms关闭A
3. 把点击事件换成touch事件

## 13. 移动端兼容性问题
### 1. 点击事件延迟300毫秒
### 2. 点透
### 3. 安卓下大面积触摸会导致触发touchmove的问题
　- 解决方案：在touchmove中判断一下touchstart的上一次位置和当前位置是否一样，一样就使move return掉
### 4. 输入框被遮挡
  - 解决方案：
    - 在focus里去做：获取屏幕高度，比较输入框的底部是否超出了屏幕，超出就让外框向上移动超 出的距离。在软件盘弹出之后（在focus中加个延迟时间），用getBoundingClientRect()获取input的距离视窗的位置，如果input底部到视窗的高度比视窗高度大，说明被遮挡，此时需要上移。
    - 在blur里做：把外框回到原来位置
### 5. 滚动卡顿
` -webkit-overflow-scrolling:touch; overflow-scrolling:touch; ` 滚动回弹
### 6. iphone及ipad下输入框默认内阴影
`-webkit-appearance:none;`

## 14. 阻塞DOM解析和渲染
- CSS 不会阻塞 DOM 的解析，但会阻塞 DOM 渲染。
- JS 阻塞 DOM 解析，但浏览器会"偷看"DOM，预先下载相关资源。
- 浏览器遇到 script标签 且没有defer或async属性的 标签时，会触发页面渲染，因而如果前面CSS资源尚未加载完毕时，浏览器会等待它加载完毕在执行脚本。
- script 最好放底部，<link>最好放头部，如果头部同时有script与<link>的情况下，最好将script放在<link>上面

## 15. 长列表渲染
（https://www.cnblogs.com/cangqinglang/p/12341362.html）
### 分析
- 消耗时间最多的两个阶段是Recalculate Style和Layout 
  1. Recalculate Style：样式计算，浏览器根据css选择器计算哪些元素应该应用哪些规则，确定每个元素具体的样式。
  2. Layout：布局，知道元素应用哪些规则之后，浏览器开始计算它要占据的空间大小及其在屏幕的位置。
  
### 方案

#### 时间分片
- 适用于列表项DOM结构比较简单
- 使用 requestAnimationFrame

#### 虚拟列表
- 虚拟列表：其实是按需显示的一种实现，即只对可见区域进行渲染，对非可见区域中的数据不渲染或部分渲染的技术，从而达到极高的渲染性能。
- 实现：在首屏加载的时候，只加载可视区域内需要的列表项，当滚动发生时，动态通过计算获得可视区域内的列表项，并将非可视区域内存在的列表项删除。

## 16. 首页白屏
### 原因：
在JS没有解析加载完成之前页面无法展示，会处于长时间的白屏

### 解决方案
1. SSR
2. 预渲染，可以使用插件prerender-spa-plugin
3. 骨架屏，可以使用插件page-skeleton-webpack-plugin
4. 服务端开启Gzip压缩

## 17. Git相关
参考（https://segmentfault.com/a/1190000019714354，https://blog.csdn.net/weixin_41910848/article/details/83989495）

- Git 生命周期。生命周期的本质是工作流程。Git工作流程如下：

	1. 从仓库中克隆一个副本作为工作库；
	2. 添加修改副本中的文件；
	3. 可选，同步其他人员修改过的工作库；
	4. 提交之前查看更改；
	5. 提交更改，将更改推送到存储库；
	6. 提交之后，如果有问题，则更正上次提交并将更改推送到存储库。
	
- 常用指令：init、clone、add、commit、push、pull、fetch、merge、checkout、branch、reset、revert、cherry-pick、status

- 日常操作：git init 创建一个Git服务器，git clone 将版本库克隆到本地，'git branch a'创建a分支，'git checkout a'切换到a分支，在当前分支进行开发，编写完成之后执行git add，git commit，git push将代码提交到远程a分支。测试通过之后将a分支合并到远程master主分支。如果a分支不是从最新的master中牵出来的，那么需要执行'git checkout master'把代码切换到master分支，'git pull origin master'将最新的远程master分支代码拉到本地master分支。'git checkout a'切换到本地a分支，'git merge master'将本地master分支合并到当前分支，如果合并过程中有冲突，手动合并之后将本地a分支代码更新到远程a分支，按顺序执行'git add -A'、'git commit -m 'xxx'、'git push origin a'。然后GitHub上发起 Pull requests 请求将 a 分支的代码合并到 master 分支。
- 另外一种模式：
本地：Relese、test、开发分支；远端Relese、test、开发分支；接到新的需求，同步本地的Relese然后迁出一条开发分支，本地开发，开发通过之后合并到本地test，再推送到远端的test，在测试环境发布、等待运营和产品检查，通过后发起上线，将本地开发分支push到远端然后发起MR，code review通过之后，合并代码，等待运营发布。

## 18. 关于html，css，js三者的加载顺序问题
### DOM加载到link标签
css文件的加载是与DOM的加载并行的，也就是说，css在加载时Dom还在继续加载构建，而过程中遇到的css样式或者img，则会向服务器发送一个请求，待资源返回后，将其添加到dom中的相对应位置中；
### DOM加载到script标签
- 由于js文件不会与DOM并行加载，因此需要等待js整个文件加载完之后才能继续DOM的加载，倘若js脚本文件过大，则可能导致浏览器页面显示滞后，这种效应称之为“阻塞效应”；
- js阻塞其他资源的加载的原因是：浏览器为了防止js修改DOM树，需要重新构建DOM树的情况出现；
### 解决方法
- 延迟加载：在script标签中添加 defer=“ture”，相当于告诉浏览器立即下载，但延迟执行。则会让js与DOM并行加载，待页面加载完!成后再执行js文件，这样则不存在阻塞；可以让脚本在文档完全呈现之后再执行,延迟脚本总是按照指定它们的顺序执行。适用在需要操控DOM元素的。
- 异步加载：在scirpt标签中添加 async=“ture”，只适用于外部脚本文件，并告诉浏览器立即下载文件，下载完成后立即执行。这个属性告诉浏览器该js文件是异步加载执行的，也就是不依赖于其他js和css，也就是说无法保证js文件的加载顺序，但是同样有与DOM并行加载的效果。建议异步脚本不要在加载期间修改 DOM。适用于与DOM无关的。
- 把scirpt标签放在<body> 元素中页面内容的后面。

## 19. XSS和CSRF
- XSS，即 Cross Site Script，中译是跨站脚本攻击。指攻击者在网站上注入恶意的客户端代码，通过恶意脚本对客户端网页进行篡改，从而在用户浏览网页时，对用户浏览器进行控制或者获取用户隐私数据的一种攻击方式。通常出现在搜索框、留言板、评论区等地方
- 分类：反射性、存储型、DOM型
  1. 反射性：发出请求时，XSS代码出现在url中，作为输入提交到服务器端，服务器端解析后响应，XSS代码随响应内容一起传回给浏览器，最后浏览器解析执行XSS代码。
  2. 存储型：存储型XSS和反射型XSS的差别在于，提交的代码会存储在服务器端（数据库、内存、文件系统等），下次请求时目标页面时不用再提交XSS代码。
  3. 基于DOM 的 XSS 攻击是指通过恶意脚本修改页面的 DOM 结构，是纯粹发生在客户端的攻击。
- 防范方法：设置黑名单和白名单、对用户输入进行过滤、入参字符过滤、出参字符转义、设置httponly
### CSRF
- CSRF，即 Cross Site Request Forgery，中译是跨站请求伪造，是一种劫持受信任用户向服务器发送非预期请求的攻击方式。攻击者借助受害者的 Cookie 骗取服务器的信任，可以在受害者毫不知情的情况下以受害者名义伪造请求发送给受攻击服务器，从而在并未授权的情况下执行在权限保护之下的操作。
- 防护方法：
  1. 设置referer字段；通过 Referer Check，可以检查请求是否来自合法的"源"。但是并不是所有服务器在任何时候都可以接受referer字段，所以这个方法存在一定的局限性
  2. 设置验证码；在用户操作的过程中设置身份确认的验证码，该方法会很有用，不过验证码频繁的话会影响用户体验
　3. 设置token值，在用户请求中设置一个随机没有规律的token值可以有效的防止攻击者利用漏洞攻击
### 区别
- xss：诱骗用户点击恶意链接盗取用户cookie进行攻击、不需要用户进行登录、xss除了利用cookie还可以篡改网页等
- csrf：无法获取用户的cookie而是直接冒充用户、需要用户登录后进行操作

# Vue.js
## 1. 运行机制
### 初始化流程（new Vue的具体流程）
（https://www.jianshu.com/p/f96a99a33742；https://juejin.im/post/6844904099704471559）
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/92fac48c74dd4e8b96def506ea32d0cb~tplv-k3u1fbpfcp-zoom-1.image)

 - 创建Vue实例对象，调用vue构造函数，先判断用户是否没有用 new Vue(); 是就警告用户必须new来实例化，不能Vue()，必须要 new Vue()。
 - 调用原型方法 this._init(options)。init过程会初始化生命周期，初始化事件中心，初始化渲染、执行beforeCreate周期函数、初始化 data、props、computed、watcher、执行created周期函数等。
 - 初始化后，调用$mount方法对Vue实例进行挂载（挂载的核心过程包括模板编译、渲染以及更新三个过程）。
 - 如果没有在Vue实例上定义render方法而是定义了template，那么需要经历编译阶段。需要先将template 字符串编译成 render function，template 字符串编译步骤如下：

    - parse正则解析template字符串形成AST（抽象语法树，是源代码的抽象语法结构的树状表现形式）
    - optimize标记静态节点跳过diff算法（diff算法是逐层进行比对，只有同层级的节点进行比对，因此时间的复杂度只有O(n))
    - generate将AST转化成render function字符串
    
 - 编译成render function 后，调用$mount的mountComponent方法，先执行beforeMount钩子函数，然后核心是实例化一个渲染Watcher，在它的回调函数（初始化的时候执行，以及组件实例中监测到数据发生变化时执行）中调用updateComponent方法（此方法调用render方法生成虚拟Node，最终调用update方法更新DOM）。
 - 调用render方法将render function渲染成虚拟的Node（真正的 DOM 元素是非常庞大的，因为浏览器的标准就把 DOM 设计的非常复杂。如果频繁的去做 DOM 更新，会产生一定的性能问题，而 Virtual DOM 就是用一个原生的 JavaScript 对象去描述一个 DOM 节点，所以它比创建一个 DOM 的代价要小很多，而且修改属性也很轻松，还可以做到跨平台兼容），render方法的第一个参数是createElement(或者说是h函数)，这个在官方文档也有说明。
 - 生成虚拟DOM树后，需要将虚拟DOM树转化成真实的DOM节点，此时需要调用update方法，update方法又会调用pacth方法把虚拟DOM转换成真正的DOM节点。需要注意在图中忽略了新建真实DOM的情况（如果没有旧的虚拟Node，那么可以直接通过createElm创建真实DOM节点），这里重点分析在已有虚拟Node的情况下，会通过sameVnode判断当前需要更新的Node节点是否和旧的Node节点相同（例如我们设置的key属性发生了变化，那么节点显然不同），如果节点不同那么将旧节点采用新节点替换即可，如果相同且存在子节点，需要调用patchVNode方法执行diff算法更新DOM，从而提升DOM操作的性能。

### 响应式流程
 - 在init的时候会利用Object.defineProperty方法（不兼容IE8）监听Vue实例的响应式数据的变化从而实现数据劫持能力（利用了JavaScript对象的访问器属性get和set，在未来的Vue3中会使用ES6的Proxy来优化响应式原理）。在初始化流程中的编译阶段，当render function被渲染的时候，会读取Vue实例中和视图相关的响应式数据，此时会触发getter函数进行依赖收集（将观察者Watcher对象存放到当前闭包的订阅者Dep的subs中），此时的数据劫持功能和观察者模式就实现了一个MVVM模式中的Binder，之后就是正常的渲染和更新流程。
 - 当数据发生变化或者视图导致的数据发生了变化时，会触发数据劫持的setter函数，setter会通知初始化依赖收集中的Dep中的和视图相应的Watcher，告知需要重新渲染视图，Wather就会再次通过update方法来更新视图。

## 2. 修饰符
### 事件修饰符
.stop
.prevent
.capture
.self
.once
.passive
### 按键修饰符
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right
### 系统修饰键
.ctrl
.alt
.shift
.meta
.exact 修饰符
### 鼠标按钮修饰符
.left
.right
.middle
### v-model修饰符
.lazy 光标离开更新
.trim 过滤首尾的空格
.number 如果你先输入数字，那它就会限制你输入的只能是数字。 如果你先输入字符串，那它就相当于没有加.number
### v-bind修饰符
.sync
.prop
.camel 驼峰显示属性

## 3. watch的实现原理
- watch的分类：

    - deep watch（深层次监听）
    - user watch（用户监听）
    - computed watcher（计算属性）
    - sync watcher（同步监听）

- watch实现过程：

    - watch的初始化在data初始化之后（此时的data已经通过Object.defineProperty的设置成响应式）
    - watch的key会在Watcher里进行值的读取，也就是立马执行get获取value（从而实现data对应的key执行getter实现对于watch的依赖收集），此时如果有immediate属性那么立马执行watch对应的回调函数
    - 当data对应的key发生变化时，触发user watch实现watch回调函数的执行


## 4. computed运行原理
- computed的属性是动态挂载到vm实例上的，和普通的响应式数据在data里声明不同
- 设置computed的getter，如果执行了computed对应的函数，由于函数会读取data属性值，因此又会触发data属性值的getter函数，在这个执行过程中就可以处理computed相对于data的依赖收集关系了
- 首次计算computed的值时，会执行vm.computed属性对应的getter函数（用户指定的computed函数，如果没有设置getter，那么将当前指定的函数赋值computed属性的getter），进行上述的依赖收集
- 如果computed的属性值又依赖了其他computed计算属性值，那么会将当前target暂存到栈中，先进行其他computed计算属性值的依赖收集，等其他计算属性依赖收集完成后，在从栈中pop出来，继续进行当前computed的依赖收集
 
## 5. nextTick
- 用法：
  在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

- 原理：
  目的就是产生一个回调函数加入task或者microtask中，当前栈执行完以后（可能中间还有别的排在前面的函数）调用该回调函数，起到了异步触发（即下一个tick时触发）的目的。

## 6. vue-router有哪几种导航守卫

Vue-router 导航守卫分别有全局守卫，独享守卫和组件内守卫，一般用的最多的就是router.beboreEach(进入路由之前) 里做权限校验，或者组件内的权限校验用的beforeRouteEnter

- 全局守卫：
    - router.beforeEach 全局前置守卫 进入路由之前
    - router.beforeResolve 全局解析守卫(2.5.0+) 在beforeRouteEnter调用之后调用
    - router.afterEach 全局后置钩子 进入路由之后

- 路由独享守卫：
    - beforeEnter

- 路由组件内守卫:
    - beforeRouteEnter 进入路由前, 在路由独享守卫后调用，不能获取组件实例this，组件实例还没被创建
    - beforeRouteUpdate (2.2) 路由复用同一个组件时, 在当前路由改变，但是该组件被复用时调用
    - beforeRouteLeave 离开当前路由时, 导航离开该组件的对应路由时调用

- 完整的导航解析流程
  1. 导航被触发。
  2. 在失活的组件里调用 beforeRouteLeave 守卫。
  3. 调用全局的 beforeEach 守卫。
  4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
  5. 在路由配置里调用 beforeEnter。
  6. 解析异步路由组件。
  7. 在被激活的组件里调用 beforeRouteEnter。
  8. 调用全局的 beforeResolve 守卫 (2.5+)。
  9. 导航被确认。
  10. 调用全局的 afterEach 钩子。
  11. 触发 DOM 更新。
  12. 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。

## 7. 权限校验
### 登陆态校验
  - beforeEach（登陆态存储在vuex中）
  
  - 向服务器发送请求
  
    登陆的时候，登陆成功后会在vuex中存储已经登陆 isLogin。为了防止页面刷新过程中vuex存储信息消失，服务器是记录登陆态的。

    - 会话方式  cookie session
    - token方式  JWT生成token  客户端把token存储起来（本地）

### 菜单/按钮/功能的权限【显示隐藏】
  - 登陆成功从服务器获取用户的权限字段，存储在本地（本地存储[不安全，可以加密存储] / vuex[推荐]）
  - 自定义指令控制渲染还是不渲染（不建议大家组件内 v-if）
  - 有的公司，客户端渲染的菜单，都是服务器返回的HTML(服务器压力比较大，但是安全)

### 不管是否有权限都能看，只不过点击会提示或者不让跳转路由，再或者跳转到指定的路由
  - beforeEach
  - 组件内自己判断

### 数据接口权限
 - 服务器处理的（保证绝对的安全性）

## 8. keep-alive(使用参考：https://juejin.im/post/5cdcbae9e51d454759351d84)
keep-alive 是 Vue  内置的一个组件，可以使被包含的组件保留状态，避免重新渲染 ，其有以下特性：
- 一般结合路由和动态组件一起使用，用于缓存组件；
- 提供 include 和 exclude 属性，两者都支持字符串或正则表达式， include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存 ，其中 exclude 的优先级比 include 高；
- 对应两个钩子函数 activated 和 deactivated ，当组件被激活时，触发钩子函数 activated，当组件被移除时，触发钩子函数 deactivated。

## 9. v-show 与 v-if 有什么区别
- 区别：
  1. v-show是css切换，会被编译成指令，条件不满足时控制样式将对应节点隐藏。
  2. v-if在编译过程中会被转化成三元表达式，条件不满足时不渲染此节点。v-if是完整的销毁和重新创建
- 建议：使用频繁切换时用v-show，运行时较少改变时用v-if

## 10. v-model 的原理
```
<input v-model='searchText'>
//=>原理
<input :value="searchText" 
    @input="searchText= $event.target.value">
```
如果需要让一个自定义组件支持 v-model=’value’ 指令实现双向数据绑定，即表示父组件调用子组件时用value属性传递了输入框值，以及绑定了一个input事件，可以通过这个事件让子组件给父组件传参。

## 11. computed 和 watch 的区别和运用的场景
### computed 
  - 定义：是计算属性，依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值；
  - 运用场景：当我们需要进行数值计算，并且依赖于其它数据时，应该使用 computed，因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算；
  - 区别：
  
  	1.computed支持缓存（只有引用的响应式属性改变时才会重新计算）

	2.computed不支持异步操作

	3.computed中的属性有一个get和set方法，如果属性为函数时，默认调用get方法。

### watch 
   - 定义：更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；
   - 运用场景：当我们需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API )，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。
   - 区别：
   
    1.watch不支持缓存，会直接监听数据，当数据改变时，会直接触发对于的操作

	2.watch支持异步操作
	
	3.watch可以接收参数（oldValue，newValue）
	
	4.watch中的属性必须时data中声明或者其他组件传递过来的数据

	5.watch中的属性为对象时，可以使用其handler方法、immediate属性（当为true时，页面刷新时就会调用handler方法）和deep属性（当为true时，能够监听复杂属性类型）

## 12. 对vue生命周期的理解
- created/mounted/updated/destroyed，以及对应的before钩子。分别是创建=>挂载=>更新=>销毁。
- activated & deactivated,  被 keep-alive 缓存的组件激活/停用时调用
- errorCaptured（v2.5 以上版本有的一个钩子，用于处理错误）
- serverPrefetch，前身是ssrPrefetch，用来处理ssr的。允许我们在渲染过程中“等待”异步数据。

## 13. Vue 怎么实现数据双向绑定的：
- 实现一个监听器Observer：
  对数据对象进行遍历，包括子属性对象的属性，利用 Object.defineProperty() 对属性都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter， 那么就能监听到了数据变化。

- 实现一个解析器 Compile：
  解析 Vue 模板指令，将模板中的变量都替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新。

- 实现一个订阅者 Watcher：
  Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁 ，主要的任务是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile 中对应的更新函数。

- 实现一个订阅器 Dep：
  订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。

## 14. 组件间相互通信的方法
1. props / $emit 适用 父子组件通信
2. $refs 与 $parent / $children 适用 父子组件通信。在父组件中的子组件添加一个ref属性，this.$refs.xxx就能获取到对应的子组件实例
3. EventBus （$emit / $on） 适用于 父子、隔代、兄弟组件通信。创建一个eventBus.js文件，在需要传参的组件中引入改文件，传参时eventBus.$emit(eventName, [...args])，接收参数的组件内eventBus.$on(eventName, callback)
4. provide / inject 适用于 隔代组件通信
5. vuex 公共状态管理 适用于SPA单页面应用中的各类情况

## 15. 怎样理解 Vue 的单向数据流？
- 所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。

- Vue 的父组件和子组件生命周期钩子函数执行顺序可以归类为以下 4 部分：
  - 加载渲染过程：父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
  - 子组件更新过程：父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated
  - 父组件更新过程：父 beforeUpdate -> 父 updated
  - 销毁过程：父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed

- 每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你不应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。子组件想修改时，只能通过 $emit 派发一个自定义事件，父组件接收到后，由父组件修改。有两种常见的试图改变一个 prop 的情形 :
  - 这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用。 在这种情况下，最好定义一个本地的 data 属性并将这个 prop 用作其初始值
  - 这个 prop 以一种原始的值传入且需要进行转换。 在这种情况下，最好使用这个 prop 的值来定义一个计算属性

## 16. 路由跳转的方式
1. router-link
2. this.$router.push()。用params传参只能用name，如果用path则用query传参或者手写带有完整参数的path.
3. this.$router.replace()，与上一个不同的是history栈中不会有记录。
4. this.$router.go(n)，向前或者向后跳转n个页面，n可为正整数或负整数。

## 17.vuex（参考：https://www.jianshu.com/p/2e5973fe1223）
- 定义：VueX是适用于在Vue项目开发时使用的状态管理工具.
- 使用场景：单页应用中，组件之间的共享状态和方法。
- 安装：
	1. npm 安装 Vuex  `npm i vuex -s`
	2. 在项目的根目录下新增一个store文件夹，在该文件夹内创建index.js
- 使用：
	1. 初始化store下index.js中的内容，export输出
	2. 在main.js中引入store， 将store挂载到当前项目的Vue实例当中去
	3. 在组件中使用Vuex `this.$store.state.xxx`就能获取到store中定义的状态xxx
- 属性：
	1. state：包含了store中存储的各个状态。
	2. getter: 类似于 Vue 中的计算属性，根据其他 getter 或 state 计算返回值。参数：(state, getters)
	3. mutation:定义的方法动态修改Vuex 的 store 中的状态或数据，只能是同步操作。参数：(state, payload)
	4. action: 由于直接在mutation方法中进行异步操作，将会引起数据失效。所以提供了Actions来专门进行异步操作，最终提交mutation方法。view 层通过 store.dispatch 来分发 action。。参数：(context, payload)
	5. modules：项目特别复杂的时候，可以让每一个模块拥有自己的state、mutation、action、getters,使得结构非常清晰，方便管理。

## 18. 路由按需引入
1. `component: () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')`
2. `component: resolve => require(['../components/PromiseDemo'], resolve)`

## 19. vue项目中遇到的问题
- 打包文件太大，使用路由懒加载，对代码进行分割，require/import；
- setInterval路由跳转继续运行并没有及时进行销毁，在beforeDestory中clearInterval；

## 20. vue插件开发

## 21. vue-router 原理
（https://www.cnblogs.com/xiaozhumaopao/p/12716266.html）

1. 实现一个静态install方法，因为作为插件都必须有这个方法，给Vue.use()去调用；
2. 可以监听路由变化；
3. 解析配置的路由，即解析router的配置项routes，能根据路由匹配到对应组件；
4. 实现两个全局组件router-link和router-view；（最终落地点）

## 23.VueJS开发所用到的技术栈
1. 主要使用vue.js
2. 使用vue-cli脚手架搭建项目
3. 使用vue-router来做路由，实现单页面跳转
4. 使用mand-mobile作为前端UI框架
5. 使用axios来请求接口，实现前后端分离
6. 使用Echarts.js来实现图表效果
7. 使用Vuex管理共享状态
8. 使用vue-loader，解析和转换 .vue 文件，提取出其中的逻辑代码 script、样式代码 style、以及 HTML 模版 template，再分别把它们交给对应的 Loader 去处理，核心的作用，就是提取，划重点。

## 24. Vue 指令
v-text、v-html、v-show、v-if、v-else、v-if-else、v-for、v-on、v-bind、v-model、v-slot、v-pre、v-cloak、v-once

# React
## 1. 高阶组件
- 定义：高阶组件（Higher Order Component，HOC）并不是React提供的某种API，而是使用React的一种模式，用于增强现有组件的功能。一个高阶组件就是一个函数，这个函数接受一个组件作为输入，然后返回一个新的组件作为结果，而且，返回的新组件拥有了输入组件所不具备的功能。这里提到的组件并不是组件实例，而是组件类，也可以是一个无状态组件的函数。
- 作用：
  1. 重用代码。
  2. 修改现有React组件的行为。
- 按实现方式分类：
  1. 代理方式的高阶组件
  2. 继承方式的高阶组件

## 2. 生命周期

- componentWillMount 在渲染前调用,在客户端也在服务端。

- componentDidMount : 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)。

- componentWillReceiveProps 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。

- shouldComponentUpdate 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。
可以在你确认不需要更新组件时使用。

- componentWillUpdate在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。

- componentDidUpdate 在组件完成更新后立即调用。在初始化时不会被调用。

- componentWillUnmount在组件从 DOM 中移除之前立刻被调用。

## 3. 父子组件嵌套时的生命周期
Parent constructor

Parent componentWillMount

Parent render

Child constructor

Child componentWillMount

Child render

Child componentDidMount

Parent componentDidMount


# webpack
## 1. 谈谈你对webpack的看法
 - 定义：WebPack 是一个模块打包工具，你可以使用WebPack管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包Web开发中所用到的HTML、Javascript、CSS以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，webpack有对应的模块加载器。webpack模块打包器会分析模块间的依赖关系，最后 生成了优化且合并后的静态资源。
 - 两大特点：
    - code splitting（可以自动完成）
    - loader 可以处理各种类型的静态文件，并且支持串联操作
- webpack 是以commonJS的形式来书写脚本滴，但对 AMD/CMD 的支持也很全面，方便旧项目进行代码迁移。
- webpack具有requireJs和browserify的功能，但仍有很多自己的新特性：
    1. 对 CommonJS、AMD、ES6的语法做了兼容
    2. 对js、css、图片等资源文件都支持打包
    3. 串联式模块加载器以及插件机制，让其具有更好的灵活性和扩展性，例如提供对CoffeeScript、ES6的支持
    4. 有独立的配置文件webpack.config.js
    5. 可以将代码切割成不同的chunk，实现按需加载，降低了初始化时间
    6. 支持 SourceUrls 和 SourceMaps，易于调试
    7. 具有强大的Plugin接口，大多是内部插件，使用起来比较灵活
    8. webpack 使用异步 IO 并具有多级缓存。这使得 webpack 很快且在增量编译上更加快

## 2. Webpack的loader和plugins的区别
- Webpack打包原理：
    - 识别入口文件
    - 通过逐层识别模块依赖。（Commonjs、amd或者es6的import，webpack都会对其进行分析。来获取代码的依赖）
    - webpack做的就是分析代码。转换代码，编译代码，输出代码
    - 最终形成打包后的代码
- loader：是文件加载器，能够加载资源文件，并对这些文件进行一些处理，诸如编译、压缩等，最终一起打包到指定的文件中。
    - 处理一个文件可以使用多个loader，loader的执行顺序是和本身的顺序是相反的，即最后一个loader最先执行，第一个loader最后执行。
    - 第一个执行的loader接收源文件内容作为参数，其他loader接收前一个执行的loader的返回值作为参数。最后执行的loader会返回此模块的JavaScript源码
- Plugin：在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
 
- 区别：
    - 对于loader，它就是一个转换器，将A文件进行编译形成B文件，这里操作的是文件，比如将A.scss或A.less转变为B.css，单纯的文件转换过程。
    - plugin是一个扩展器，它丰富了wepack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点，执行广泛的任务。

## 3. webpack提高打包速度
（参考：https://blog.csdn.net/weixin_40811829/article/details/88599201；https://blog.csdn.net/xiaoenwu/article/details/106698039）
### 1. 使用 DLLPlugin和DLLReferencePlugin两个插件 提高打包速度
  - 单独对那些不常更新的第三方模块进行打包，使第三方模块只打包一次，以后的每次构建都只会生成自己的业务代码，可以很好的提高构建效率。
### 2. 优化loader配置
  - 缩小文件匹配范围(include/exclude)，exclude用于排除不处理的目录，通过排除node_modules下的文件 从而缩小了loader加载搜索范围 高概率命中文件；
  - 缓存loader的执行结果(cacheDirectory)。
### 3. resolve优化配置
  - 优化模块查找路径 resolve.modules。为了减少搜索范围，可以直接写明 node_modules 的全路径。
  -  resolve.alias 配置路径别名。配置项通过别名来把原导入路径映射成一个新的导入路径 此优化方法会影响使用Tree-Shaking去除无效代码。
  -  resolve.extensions，当引入模块时不带文件后缀 webpack会根据此配置自动解析确定的文件后缀，后缀列表尽可能小，频率最高的往前放，导出语句尽可能带上后缀。
### 4. module.noParse
  - 对于引入的大型第三方库，不需要将其解析成语法树来解析依赖，提高构建速度。被定义在该配置中的模块中，不能调用import/require/define等引入机制(没采用模块化)。例如jquery.
### 5. HappyPack
  - 让webpack对loader的执行过程，从单一进程形式扩展为多进程模式，也就是将任务分解给多个子进程去并发的执行，子进程处理完后再把结果发送给主进程。从而加速代码构建 与 DLL动态链接库结合来使用更佳。
### 6. ParallelUglifyPlugin
  - 这个插件可以帮助有很多入口点的项目加快构建速度。把对JS文件的串行压缩变为开启多个子进程并行进行uglify。
### 7. terser-webpack-plugin
  - 对js进行压缩的插件
### 8. 模块化引入
```
import {chain, cloneDeep} from 'lodash';
// 可以改写为
import chain from 'lodash/chain';
import cloneDeep from 'lodash/cloneDeep';
```
### 9. 开启Gzip压缩


## 4. 不同环境的配置
- 在config文件下创建不同环境的配置文件，开发dev.config.js/生产prod.config.js
- 在package.json中的scripts属性中配置开发打包命令和上线打包命令，dev对应开发，build对应生产，指定不同的配置文件。


# 浏览器
## 1. 讲讲304缓存的原理
- 服务器首先产生ETag，服务器可在稍后使用它来判断页面是否已经被修改。本质上，客户端通过将该记号传回服务器要求服务器验证其（客户端）缓存。

- 304是HTTP状态码，服务器用来标识这个文件没修改，不返回内容，浏览器在接收到个状态码后，会使用浏览器已缓存的文件。

- 客户端请求一个页面（A）。 服务器返回页面A，并在给A加上一个ETag。 客户端展现该页面，并将页面连同ETag一起缓存。 客户再次请求页面A，并将上次请求时服务器返回的ETag一起传递给服务器。 服务器检查该ETag，并判断出该页面自上次客户端请求之后还未被修改，直接返回响应304（未修改——Not Modified）和一个空的响应体。


## 2. 请求头的content-type有几种类型
    Content-Type（内容类型），一般是指网页中存在的 Content-Type，用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件

application/x-www-form-urlencoded、multipart/form-data、application/json、text/xml
    
## 3. TCP传输的三次握手四次挥手策略
- 为了准确无误地把数据送达目标处，TCP协议采用了三次握手策略。用TCP协议把数据包送出去后，TCP不会对传送 后的情况置之不理，它一定会向对方确认是否成功送达。握手过程中使用了TCP的标志：SYN和ACK。
- 发送端首先发送一个带SYN标志的数据包给对方。接收端收到后，回传一个带有SYN/ACK标志的数据包以示传达确认信息。 最后，发送端再回传一个带ACK标志的数据包，代表“握手”结束。 若在握手过程中某个阶段莫名中断，TCP协议会再次以相同的顺序发送相同的数据包。
- 断开一个TCP连接则需要“四次挥手”：
 1. 第一次挥手：主动关闭方发送一个FIN，用来关闭主动方到被动关闭方的数据传送，也就是主动关闭方告诉被动关闭方：我已经不 会再给你发数据了(当然，在fin包之前发送出去的数据，如果没有收到对应的ack确认报文，主动关闭方依然会重发这些数据)，但是，此时主动关闭方还可 以接受数据。
 2. 第二次挥手：被动关闭方收到FIN包后，发送一个ACK给对方，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号）。
 3. 第三次挥手：被动关闭方发送一个FIN，用来关闭被动关闭方到主动关闭方的数据传送，也就是告诉主动关闭方，我的数据也发送完了，不会再给你发数据了。
 4. 第四次挥手：主动关闭方收到FIN后，发送一个ACK给被动关闭方，确认序号为收到序号+1，至此，完成四次挥手。

## 4. 进程和线程
- 浏览器（多进程）包含了1 个浏览器（Browser）主进程、1 个 GPU 进程、1 个网络（NetWork）进程、多个渲染进程和多个插件进程。其中渲染进程和Web前端密切相关，包含以下线程：
	- GUI渲染线程
	- JS引擎线程
	- 事件触发线程（和EventLoop密切相关）
	- 定时触发器线程
	- 异步HTTP请求线程
- 注意：GUI渲染线程和JS引擎线程是互斥的，为了防止DOM渲染的不一致性，其中一个线程执行时另一个线程会被挂起。
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/93caa132594d427781b8c186e32df1e5~tplv-k3u1fbpfcp-zoom-1.image)


## 5. Cookie、Session、Token、JWT (参考：https://juejin.im/post/5e055d9ef265da33997a42cc)
### Cookie
- HTTP 是无状态的协议，需要通过 cookie 或者 session 去维护一个状态。cookie 存储在客户端，cookie 是不可跨域的。
- cookie的重要属性：name=value，domain，path，maxAge，expires，secure，httpOnly

### Session
- session 是另一种记录服务器和客户端会话状态的机制
- session 是基于 cookie 实现的，session 存储在服务器端，sessionId 会被存储到客户端的cookie 中

### Cookie 和 Session 的区别
- 安全性： Session 比 Cookie 安全，Session 是存储在服务器端的，Cookie 是存储在客户端的。
- 存取值的类型不同：Cookie 只支持存字符串数据，想要设置其他类型的数据，需要将其转换成字符串，Session 可以存任意数据类型。
- 有效期不同： Cookie 可设置为长时间保持，比如我们经常使用的默认登录功能，Session 一般失效时间较短，客户端关闭（默认情况下）或者 Session 超时都会失效。
- 存储大小不同： 单个 Cookie 保存的数据不能超过 4K，Session 可存储数据远高于 Cookie，但是当访问量过多，会占用过多的服务器资源。
 

### Token（令牌）
1. Acesss Token
- 访问资源接口（API）时所需要的资源凭证
- 简单 token 的组成： uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign（签名，token 的前几位以哈希算法压缩成的一定长度的十六进制字符串）
- 特点：
    - 服务端无状态化、可扩展性好
    - 支持移动端设备
    - 安全
    - 支持跨程序调用
- 每一次请求都需要携带 token，需要把 token 放到 HTTP 的 Header 里
- 基于 token 的用户认证是一种服务端无状态的认证方式，服务端不用存放 token 数据。用解析 token 的计算时间换取 session 的存储空间，从而减轻服务器的压力，减少频繁的查询数据库
- token 完全由应用管理，所以它可以避开同源策略

2. Refresh Token
- refresh token 是专用于刷新 access token 的 token
- Access Token 的有效期比较短，当 Acesss Token 由于过期而失效时，使用 Refresh Token 就可以获取到新的 Token，如果 Refresh Token 也失效了，用户就只能重新登录了。
- Refresh Token 及过期时间是存储在服务器的数据库中，只有在申请新的 Acesss Token 时才会验证，不会对业务接口响应时间造成影响，也不需要向 Session 一样一直保持在内存中以应对大量的请求。

### Token 和 Session 的区别
- Session 是一种记录服务器和客户端会话状态的机制，使服务端有状态化，可以记录会话信息。而 Token 是令牌，访问资源接口（API）时所需要的资源凭证。Token 使服务端无状态化，不会存储会话信息。
- Session 和 Token 并不矛盾，作为身份认证 Token 安全性比 Session 好，因为每一个请求都有签名还能防止监听以及重放攻击，而 Session 就必须依赖链路层来保障通讯安全了。如果你需要实现有状态的会话，仍然可以增加 Session 来在服务器端保存一些状态。
- 如果你的用户数据可能需要和第三方共享，或者允许第三方调用 API 接口，用 Token 。如果永远只是自己的网站，自己的 App，用什么就无所谓了。

### JWT
- JSON Web Token（简称 JWT）是目前最流行的跨域认证解决方案。
- 是一种认证授权机制。
- JWT 认证流程：
  - 用户输入用户名/密码登录，服务端认证成功后，会返回给客户端一个 JWT
  - 客户端将 token 保存到本地（通常使用 localstorage，也可以使用 cookie）
  - 当用户希望访问一个受保护的路由或者资源的时候，需要请求头的 Authorization 字段中使用Bearer 模式添加 JWT，其内容看起来是下面这样

    `Authorization: Bearer <token>`
    
- JWT 的特点：
  - 服务端的保护路由将会检查请求头 Authorization 中的 JWT 信息，如果合法，则允许用户的行为
  - 因为 JWT 是自包含的（内部包含了一些会话信息），因此减少了需要查询数据库的需要
  - 因为 JWT 并不使用 Cookie 的，所以你可以使用任何域名提供你的 API 服务而不需要担心跨域资源共享问题（CORS）
  - 因为用户的状态不再存储在服务端的内存中，所以这是一种无状态的认证机制。

- JWT 的使用方式

  客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。
  
  1. 方式一：当用户希望访问一个受保护的路由或者资源的时候，可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求头信息的 Authorization 字段里，使用 Bearer 模式添加 JWT。
  2. 方式二：跨域的时候，可以把 JWT 放在 POST 请求的数据体里。
  3. 方式三：通过 URL 传输
  

### Token 和 JWT 的区别

- 相同：
  - 都是访问资源的令牌
  - 都可以记录用户的信息
  - 都是使服务端无状态化
  - 都是只有验证成功后，客户端才能访问服务端上受保护的资源
- 不同：
  - Token：服务端验证客户端发送过来的 Token 时，还需要查询redis获取用户信息，然后验证 Token 是否有效。
  - JWT： 将 Token 和 Payload 加密后存储于客户端，服务端只需要使用密钥解密进行校验（校验也是 JWT 自己实现的）即可，不需要查询或者减少查询数据库，因为 JWT 自包含了用户信息和加密的数据。

## 6. HTTP状态码206是干什么的
 206状态码表示客户端通过发送范围请求头Range抓取到了资源的部分数据，一般用来解决大文件下载问题，一般CDN服务器都会支持这种能力。
 
## 7. Http2和Http1.X的区别
1. 区别：
   - HTTP2使用的是二进制传送，HTTP1.X是文本（字符串）传送。二进制传送的单位是帧和流。帧组成了流，同时流还有流ID标示。
   - HTTP2支持多路复用。即连接共享，即每一个request都是是用作连接共享机制的。一个request对应一个id，这样一个连接上可以有多个request，每个连接的request可以随机的混杂在一起，接收方可以根据request的 id将request再归属到各自不同的服务端请求里面。
   - HTTP2引入了头信息压缩机制，通过gzip和compress压缩头部然后再发送，同时客户端和服务器端同时维护一张头信息表，所有字段都记录在这张表中，这样后面每次传输只需要传输表里面的索引Id就行，通过索引ID查询表头的值。
   - HTTP2支持服务器推送。
2. http2实现了多路复用,http1.x为什么不能多路复用？
   - HTTP 1.1是基于文本分割解析的协议,也没有序号,如果多路复用会导致顺序错乱,http2则用帧的方式,等于切成一块块,每一块都有对应的序号,所以可以实现多路复用.

## 8. get和post的区别
- GET产生一个TCP数据包；POST产生两个TCP数据包。但并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次。
  - 对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
  - 而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。
- GET在浏览器回退时是无害的，而POST会再次提交请求。
- GET产生的URL地址可以被Bookmark，而POST不可以。
- GET请求会被浏览器主动cache，而POST不会，除非手动设置。
- GET请求只能进行url编码，而POST支持多种编码方式。
- GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
- GET请求在URL中传送的参数是有长度限制的，而POST么有。
- 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
- GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
- GET参数通过URL传递，POST放在Request body中。

## 9. 回流和重绘
- 回流：通过构造渲染树，将可见DOM节点和它对应样式结合起来，计算它们在设备视口(viewport)内的位置和大小，这个计算的阶段就是回流。
- 重绘：根据渲染树以及回流得到的几何信息，将渲染树的每个节点转换为屏幕上的实际像素，这个阶段就是重绘节点。
- 何时发生回流：
   - 添加或删除可见的DOM元素
   - 元素的位置发生变化
   - 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）
   - 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代。
   - 页面一开始渲染的时候（这肯定避免不了）
   - 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）
- 如何避免触发回流和重绘
   - 避免使用table布局。
   - 尽可能在DOM树的最末端改变class。
   - 避免设置多层内联样式。
   - 将动画效果应用到position属性为absolute或fixed的元素上
   - 避免使用CSS表达式（例如：calc()）
   - CSS3硬件加速（GPU加速）
   - 避免频繁操作样式，最好一次性重写style属性，或者将样式列表定义为class并一次性更改class属性
   - 避免频繁操作DOM，创建一个documentFragment，在它上面应用所有DOM操作，最后再把它添加到文档中
   - 也可以先为元素设置display: none，操作结束后再把它显示出来。因为在display属性为none的元素上进行的DOM操作不会引发回流和重绘
   - 避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。

## 10. localstorage sessionstorage和cookie的区别

### 基本概念及使用场景

cookie：它的主要用于保存登陆信息，比如登陆某个网站市场可以看到'记住密码’，这就是通过在cookie中存入一段辨别用户身份的数据来实现的。判断用户是否登录过网站，以便实现下次自动登录或记住密码；保存事件信息等

sessionStorage：会话，是可以将一部分数据在当前会话中保存下来，刷新页面数据依旧存在。但是页面关闭后，sessionStorage中的数据就会被清空。敏感账号一次性登录；单页面用的较多（sessionStorage 可以保证打开页面时 sessionStorage 的数据为空）

localStorage：常用于长期登录（判断用户是否已登录），适合长期保存在本地的数据。

### 区别

1. 存储大小
   - cookie：一般不超过4K（因为每次http请求都会携带cookie、所以cookie只适合保存很小的数据，如会话标识）
   - sessionStorage：5M或者更大
   - localStorage：5M或者更大
2. 数据有效期
   - cookie：一般由服务器生成，可以设置失效时间；若没有设置时间，关闭浏览器cookie失效，若设置了时间，cookie就会存放在硬盘里，过期才失效
   - sessionStorage：仅在当前浏览器窗口关闭之前有效，关闭页面或者浏览器会被清除
   - localStorage：永久有效，窗口或者浏览器关闭也会一直保存，除非手动永久清除，因此用作持久数据
3. 作用域
   - cookie：在所有同源窗口中都是共享的
   - sessionStorage：在同一个浏览器窗口是共享的（不同浏览器、同一个页面也是不共享的）
   - localStorage：在所有同源窗口中都是共享的
4. 通信
   - cokie：十种携带在同源的http请求中，即使不需要，故cookie在浏览器和服务器之间来回传递；如果使用cookie保存过多数据会造成性能问题
   - sessionStorage：仅在客户端（即浏览器）中保存，不参与和服务器的通信；不会自动把数据发送给服务器，仅在本地保存
   - localStorage：仅在客户端（即浏览器）中保存，不参与和服务器的通信；不会自动把数据发送给服务器，仅在本地保存
5. 易用性
   - cookie：需要自己进行封装，原生的cookie接口不够友好
   - sessionStorage：原生接口可以接受，可以封装来对Object和Array有更好的支持
   - localStorage：原生接口可以接受，可以封装来对Object和Array有更好的支持

## 11. 从输入URL到页面呈现发生了什么
1. 查找强缓存
2. DNS解析
3. TCP连接
4. 发送http请求
5. 关闭TCP连接
6. 浏览器渲染


# node.js

## 1.node.js的优缺点
### 优点：
1. 事件驱动，通过闭包很容易实现客户端的生命活期。
2. 不用担心多线程，锁，并行计算的问题
3. V8引擎速度非常快
4. 对于游戏来说，写一遍游戏逻辑代码，前端后端通用
### 缺点：
1. nodejs更新很快，可能会出现版本兼容
2. nodejs还不算成熟，还没有大制作
3. nodejs不像其他的服务器，对于不同的链接，不支持进程和线程操作