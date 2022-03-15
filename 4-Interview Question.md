# HTML类

## web worker

>### 1.1、web worker是什么？
>
>在 HTML 页面中，如果在执行脚本时，页面的状态是不可相应的，直到脚本执行完成后，页面才变成可相应。web worker 是运行在后台的` js`，独立于其他脚本，不会影响页面的性能。 并且通过 `postMessage` 将结果回传到主线程。这样在进行复杂操作的时候，就不会阻塞主线程了。

>### 1.2、web worker的使用
>
>* Web worker是HTML5提供的一个JavaScript多线程解决方案，可以将一些大计算量的代码交由给运行而不冻结用户界面
>* 但是子线程完全受主线程控制，且不得操作DOM，因此这个新标准并没有改变JavaScript单线程的本质
>* 使用：（1）创建在分线程执行的`js`文件  （2）在主线程中的`js`中发消息并设置回调
>
>```html
>//主线程
><input type="text" placeholder="数值" id="number">
><button id="btn">计算</button>
><script type="text/javascript">
>
>var input = document.getElementById('number')
>document.getElementById('btn').onclick = function(){
>    var number = input.value
>
>    //创建一个worker对象
>    var worker = new Worker('worker.js')
>    //绑定接收消息的监听
>    worker.onmessage = function(event){
>        console.log('主线程接收分线程返回的数据：' + event.data)
>        alert(event.data)
>    }
>}
>
>worker.postMessage(number)
>console.log('主线程向分线程发送消息' + event.number)
>
></script>
>```
>
>```js
>//分线程
>function fibonacci(n) {
>  return n<=2? 1:fibonacci(n-1) + fibonacci(n-2)
>}
>
>var onmessage = function (event) {
>  var number = event.data
>  console.log('分线程接收到主线程发送的数据' + number)
>  //计算
>  var result = fibonacci(number)
>  postMessage(result)
>  console.log('分线程向主线程返回数据' + result)
>}
>```
>
>
>### 1.3、相关API
>
>* `Worker`: 构造函数, 加载分线程执行的`js`文件
>* `Worker.prototype.onmessage`: 用于接收另一个线程的回调函数
>* `Worker.prototype.postMessage`: 向另一个线程发送消息
>
>
>### 1.4、不足
>
>* `worker`内代码不能操作DOM(更新UI), 因为这里的this不是window下
>
>* 不能跨域加载JS
>
>* 不是每个浏览器都支持这个新特性

# JavaScript类

## new操作符实现原理

>`new`操作符通过执行自定义构造函数或者`js`内置构造函数，从而生成一个实例对象
>- 创建一个空对象`let obj = new Object()`
>
>- 原型与原型链的关系(链接到原型) `obj.__proto__ = Person.prototype`
>
>- 绑定this指向，执行构造函数`let result = Person.call(obj)`
>
>- 确保返回的是一个对象(返回新对象) - 对构造函数有返回值的处理判断(基本类型忽略，引用类型输出)
>
>  ```js
>  if(typeof(result) === "object"){
>  	person1 = result //引用类型
>  }else{
>  	person1 = obj;  //值类型，原本的值类型
>  }
>  
>  另外一种完整写法：
>  function create(fn,...args){
>      //1. 创建一个空的对象
>      var obj = {};
>      //2. 将空对象的原型指向构造函数的原型
>      Object.setPrototypeOf(obj,fn.prototype)
>      //3. 将空对象作为构造函数的上下文(改变this值)
>      var result = fn.apply(obj,args)
>      //4. 对构造函数有返回值的处理判断
>      return result instanceof Object? result:obj
>  }
>  ```
>
## 深拷贝和浅拷贝
>实现深拷贝和浅拷贝的api: https://juejin.cn/post/6844904197595332622#heading-1
>
>浅拷贝:
>
>- `Object.assign()`
>
>- 扩展运算符(原理其实也是`Object.assign()`)
>
>- `Array.prototype.concat()`
>
>- `Array.prototype.slice()`
>
>深拷贝:
>
>- `JSON.parse(JSON.stringify())`- 利用JSON.stringify将对象转成JSON字符串，再用JSON.parse把字符串解析成对象，一去一来，新的对象产生了，而且对象会开辟新的栈，实现深拷贝。**这种方法虽然可以实现数组或对象深拷贝,但不能处理函数和正则**，因为这两者基于JSON.stringify和JSON.parse处理后，得到的正则就不再是正则（变为空对象），得到的函数就不再是函数（变为null）
>- 手写递归方法：**遍历对象、数组直到里边都是基本数据类型，然后再去复制，就是深度拷贝**（递归调用`deepClone`方法`cloneObj[key] = deepClone(obj[key], hash)`- 原理其实也是`JSON.parse(JSON.stringify()`）
>
## 见过哪些函数？他们的this指向
>- 普通函数(this指向window)
>- 构造函数（this指向实例化的对象）
>- 回调函数（指向window，例如setTimeout）
>- 箭头函数(没有自己的this,指向自身作用域上一层的this)
>- 匿名函数的this指向window(其实就是闭包)
>- 事件绑定函数（this 指向绑定事件）
>
>```js
>var but = document.querySelector("button");
>but.onclick = function() {
> 	console.log(this) //指向btn
> }
> ```
> 
> - generator函数(this不生效，因为Generator函数会返回遍历器对象，而不是实例对象，因此无法获取到this指向的实例对象上的私有属性和方法。但是这个遍历器对象可以继承Generator函数的prototype原型对象上的属性和方法(公有属性和方法)。如果希望修复this指向性问题，可以使用call方法将函数执行时所在的作用域绑定到Generator.prototype原型对象上。这样做，会使私有属性和方法变成公有的了，因为都在原型对象上了) - https://www.cnblogs.com/unclekeith/p/8325465.html
## 迭代数组或对象的api
>- for, while
>- forEach(forEach无法遍历对象, forEach里面接收一个回调函数，function默认有三个参数: item, index, array)
>- for in （可用于遍历数组和对象, 但它输出的只是数组的索引和对象的key, 我们可以通过索引和key取到对应的值）
>- for of
>- map: `array.map(callback[,thisArg])` map不改变原数组 `var arr = [1,2,3] var firearr = arr.map(current => current * 5)`
>- filter - 不改变原数组
>- `every`
>- `somr`
>- https://juejin.cn/post/6844903538175262734#heading-8


# ES6
## var、let、const的区别
>- var具有变量提升的机制；let和const没有变量提升的机制
>- var可以重复赋值(多次声明同一个变量)；let和const不能多次声明同一个变量
>- var和let一般声明变量；const一般声明数组和对象，不能对基本数据类型修改，只能修改数组和变量
>- var声明的变量没有自身作用域；但是let和const声明的变量有自身的作用域(块级作用域)

##  箭头函数和普通函数的区别
>- this指向问题：箭头函数的this在箭头函数定义时就决定了，而且不可修改(call, apply, bind)；箭头函数不会创建自己的this， 所以它没有自己的this，它只会在自己作用域的上一层继承this（箭头函数的this指向定义时候、是**外层第一个普通函数的this**）
>- 箭头函数不能new
>- 箭头函数没有prototype
>- 箭头函数没有arguments
>

# Webpack
## Webpack的作用
>- **模块打包**：可以将不同模块的文件打包整合在一起，并且保证它们之间的引用正确，执行有序。利用打包我们就可以在开发的时候根据我们自己的业务自由划分文件模块，保证项目结构的清晰和可读性。
>- **编译兼容**：可以通过`webpack`的`Loader`机制，帮助我们对代码做`polyfill`，还可以编译转换诸如`.less, .vue, .jsx`这类在浏览器无法识别的格式文件，让我们在开发的时候可以使用新特性和新语法做开发，提高开发效率
>- **能力扩展**：通过`webpack`的`Plugin`机制，我们在实现模块化打包和编译兼容的基础上，可以进一步实现诸如按需加载，代码压缩等一系列功能，帮助我们进一步提高自动化程度，工程效率以及打包输出的质量
## 常用的loader
>- `css-loader`：加载 CSS，支持模块化、压缩、文件导入等特性
>- `less-loader`：将less转换成CSS
>- `style-loader`：将处理好的 css 通过 style 标签的形式添加到页面（原理：动态添加 style 标签的方式，将样式引入页面）
>
>```js
>const content = `${样式内容}`
>const style = document.createElement('style'); //也相当于style.innerHTML = content;
>```
>- `babel-loader`：把 ES6 转换成 ES5
>- `eslint-loader`：通过 ESLint 检查 JavaScript 代码
>- 图片处理loader：
>   - `file-loader`：把图片输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 
>   - `url-loader`：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
>   - `image-loader`：加载并且压缩图片文件
## 常用的Plugin
>- `html-webpack-plugin`：自动引入打包输出的所有资源(JS/CSS)
>- ` clean-webpack-plugin`：自动清空打包目录（每次打包的时候，打包目录都会遗留上次打包的文件，为了保持打包目录的纯净，需要在打包前将打包目录清空）
>- `mini-css-extract-plugin`：将CSS的样式从JS中分离出来，类似以前的style-loader
>- `purgecss-webpack-plugin`:会单独提取 CSS 并清除用不到的 CSS
>- `optimize-css-assets-webpack-plugin`：压缩CSS

## Loader和Plugin的区别
>区别1：
>
>- `Loader` 本质是一个函数，在这个函数中对接收到的内容进行转换，返回转换后的结果。 相当于将Webpack不认识的内容转化为认识的内容
>
>- `Plugin` 可以扩展 Webpack 的功能，贯穿 Webpack 打包的生命周期，执行不同的任务（在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果）
>
>区别2：
>- `Loader` 在 module.rules 中配置，它的类型为数组。每一项都是一个 Object对象，内部包含了 test(类型文件)、loader、options (参数)等属性
>- `Plugin` 在 plugins 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入
>
## Webpack构建流程
>- **初始化**：读取与合并配置参数，加载 Plugin，创建`Compiler`对象并解析项目
>- **编译**：从 Entry 出发，针对每个 Module 调用对应的 Loader 去翻译文件的内容，再找到该 Module 依赖的 Module，递归地进行编译处理，最终转为`Javascript`文件
>- **输出**：将编译后的 Module 组合成 Chunk，将 Chunk 转换成文件，输出到文件系统中

## source map是什么
>`source map` 是将构建打后的代码（编译、打包、压缩）映射回源代码的过程。打包压缩后的代码不具备良好的可读性，想要调试源码就需要 soucre map
>
>| 参数   | 参数解释                                                     |
>| ------ | ------------------------------------------------------------ |
>| eval   | 打包后的模块都使用 `eval()` 执行，行映射可能不准；不产生独立的 map 文件 |
>| cheap  | map 映射只显示行不显示列，忽略源自 loader 的 source map      |
>| inline | 映射文件以 base64 格式编码，加在 bundle 文件最后，不产生独立的 map 文件 |
>| module | 增加对 loader source map 和第三方模块的映射                  |
>
>生产环境可以使用`source-map`或者`cheap-module-source-map`
>开发环境可以使用`eval-source-map`或者`eval-cheap-module-source-map`
>
>易混概念：https://juejin.cn/post/6844904007362674701
## 热更新原理
>也叫`HMR` -  可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块
>
>原理：建立起浏览器端和服务器端之间的通信，浏览器会接收服务器端推送的消息，如果需要热更新，浏览器发起http请求去服务器端获取打包好的资源解析并局部刷新页面
>
>- 通过使用`devserver`去启动本地服务，实现当浏览器访问资源时做出响应
>- webpack和浏览器会通过`websocket`实现双向连接；webpack会监听源文件的变化，当开发者保存文件时会触发webpack的重新编译（每次编译都会生成`hash值`），编译完成后会向客户端推送更新，让客户端与上一次资源进行对比
>- 客户端的`websocket`监听到有文件改动推送过来的`hash戳`，会和上一次对比，一致则走缓存，不一致则通过`ajax`和`jsonp`向服务端获取最新资源
## 文件监听原理
>作用：发现源码发生变化时，自动重新构建出新的输出文件
>
>原理：轮询判断文件的最后编辑时间是否变化，如果某个文件发生了变化，并不会立刻告诉监听者，而是先缓存起来，等 `aggregateTimeout` 后再执行。
>
>Webpack开启监听模式，有两种方式：
>
>- 启动 webpack 命令时，带上 --watch 参数
>- 在配置 webpack.config.js 中设置 watch:true
>
>缺点：每次需要手动刷新浏览器

## 文件指纹
>文件指纹是打包后输出的文件名的后缀。
>
>- `Hash`：和整个项目的构建相关，只要项目文件有修改，整个项目构建的 hash 值就会更改
>- `Chunkhash`：和 Webpack 打包的 chunk 有关，根据不同的入口文件(Entry)进行依赖文件解析、构建对应的 chunk，生成对应的哈希值
>- `Contenthash`：根据文件内容来定义 hash，文件内容不变，则 contenthash 不变（比如index.js和它对应的css文件属于同一个Chunkhash，但是如果改成Contenthash，只修改js的内容，css的Contenthash不变）
>

## 如何保证各个loader按照预想方式工作
>可以使用 `enforce` 强制执行 `loader` 的作用顺序，`pre` 代表在所有正常 loader 之前执行，`post` 是所有 loader 之后执行
>
## 代码分割
>就是将代码分离到每个 bundles/chunks 里面，可以按需加载，而不是加载一个包含全部的 bundle

## Babel原理
>**解析**：将代码转换成 AST
>
>- 词法分析：将代码(字符串)分割为token流，即语法单元成的数组
>- 语法分析：分析token流(上面生成的数组)并生成 AST
>
>**转换**：访问 AST 的节点进行变换操作生产新的 AST
>
>- [Taro](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FNervJS%2Ftaro%2Fblob%2Fmaster%2Fpackages%2Ftaro-transformer-wx%2Fsrc%2Findex.ts%23L15)就是利用 babel 完成的小程序语法转换
>
>**生成**：以新的 AST 为基础生成代码


# VUE类

vue的两大核心-组件化&数据驱动

vue的渲染有两条线，一条是初始化更新，另一条是更新
## 1. Vue的基本原理
>类似问题： 你对Vue的理解？解释下vue是什么？vue的响应式原理是什么？
>
>**总结来说**：Vue是一个框架，遵循MVVM架构，能够实现响应数据绑定和视图更新。它的基本原理是采用数据劫持结合发布-订阅模式,在vue2中通过 `Object.defineproperty` 来劫持data中各个属性的 `setter和getter`,在数据变动时发布消息给订阅者,触发响应的监听回调。
>
>**具体来说**：
>
>- 需要observe类来遍历循环data中的数据, 为每个属性都加上`setter和getter`，从而实现劫持并监听所有属性
>- 需要Dep进行依赖的收集, 在内部追踪相关依赖（也就是在Dep中存储相关的`Watcher`(把watcher push进数组里面)；在属性被修改时触发`notice()`方法通知变化。
>- 需要`Watcher`观察数据(或表达式)的变化，当调用`setter`时触发`Updated()`方法重新计算，并触发`Compile`绑定的回调，让组件视图更新变化。
>  *Watcher订阅者是Observer和Compile之间通信的桥梁，主要做的事情是: 
>   ①在自身实例化时往属性订阅器(dep)里面添加自己 
>   ②自身必须有一个update()方法 
>   ③待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调*
>- 需要`compile`解析模板，将模板中的变量替换成数据，然后初始化渲染页面视图
>
>MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。
>

## 2. MVC和MVVM的区别
>- MVC: 通过分离 Model、View 和 Controller 的方式来组织代码结构。其中 View 负责页面的显示逻辑，Model 负责存储页面的业务数据，以及对相应数据的操作。并且 View 和 Model 应用了观察者模式，当 Model 层发生改变的时候它会通知有关 View 层更新页面。Controller 一般是在后台，它主要负责用户与应用的响应操作，当用户与页面产生交互的时候，Controller 中的事件触发器就开始工作，通过调用 Model 层，来完成对 Model 的修改，然后 Model 层再去通知View层更新。
>
>缺点：（1）前后端无法独立开发，必须等接口做好 （2）前端不够独立，没有自己的数据中心，太过于依赖后台
>
>- MVVM： Model、View、ViewModel：
> （1）Model代表数据模型，数据和业务逻辑都在Model层中定义(data)
> （2）View代表UI视图，负责数据的展示(CSS和HTML)
> （3）ViewModel负责监听Model中数据的改变并且控制视图的更新，处理用户交互操作
>
>Model和View并无直接关联，而是通过ViewModel来进行联系的，Model和ViewModel之间有着双向数据绑定的联系(双向数据绑定原理)。因此当Model中的数据改变时会触发View层的刷新，View中由于用户交互操作而改变的数据也会在Model中同步。
>

## 3. computed和watch的区别
>- 缓存：computed的值有缓存，只有它依赖的属性值发生变化时，才会重新计算；而watch值没有缓存，当数据变化时他就会触发相应的操作
>- 异步操作：computed不支持异步操作，watch支持异步操作
>- watch可以设定更多的配置，它有两个的参数：immediate：组件加载立即触发回调函数；deep：深度监听，发现数据内部的变化，在复杂数据类型中使用，例如数组中的对象发生变化。需要注意的是，deep无法监听到数组和对象内部的变化。
>
>**运用场景：**
>
>- 当需要进行数值计算,并且依赖于其它数据时，应该使用 computed，因为可以利用 computed 的缓存特性，避免每次获取值时都要重新计算。
>
>- 当需要在执行异步操作时，应该使用 watch
>
>  **补充：computed的实现原理**
>computed 本质是一个惰性求值的观察者。computed 内部实现了一个惰性的 watcher,也就是 computed watcher,computed watcher 不会立刻求值,同时持有一个 dep 实例。其内部通过 this.dirty 属性标记计算属性是否需要重新求值。当 computed 的依赖状态发生改变时,就会通知这个惰性的 watcher,computed watcher 通过 this.dep.subs.length 判断有没有订阅者,有的话,会重新计算,然后对比新旧值,如果变化了,会重新渲染。 (Vue 想确保不仅仅是计算属性依赖的值发生变化，而是当计算属性最终计算的值发生变化时才会触发渲染 watcher 重新渲染，本质上是一种优化。)没有的话,仅仅把 this.dirty = true。 (当计算属性依赖于其他数据时，属性并不会立即重新计算，只有之后其他地方需要读取属性的时候，它才会真正计算，即具备 lazy（懒计算）特性。)


## 4. data为什么是函数式
> Vue组件实际上在`defineReactive`中就形成了闭包，这样每个对象的每个属性就能保存自己的值`value`和依赖对象`dep`; 就能让每一个组件都有自己的私有作用域，确保各组件数据不会相互干扰
>
> 如果是纯对象的话就会有干扰`let obj = {}`，因为当多个实例引用同一个对象时，只要一个实例对这个对象进行操作，其他实例中的数据也会发生变化。

## 5. v-if和v-show的区别
>`v-if`：不满足条件的不会渲染dom，常用于单次判断
>`v-show`:实际上是调用css样式中的display，当为false时会变成`display:none`从而隐藏dom,但是dom节点仍然存在，常用于多次切换(但不能用于权限操作)
>

## 6. 使用Object.defineProperty() 来进行数据劫持的缺点
>这种方法在对一些属性进行操作时，无法实现响应式：
>
>**针对对象来说**：只能实现对象的深层次查找和修改，而对于对象的增加和删除对象，不能触发组件的重新渲染，因为 `Object.defineProperty` 不能拦截到这些操作。
>
>**针对数组来说**：无法监听到数组的变化(例如：修改数组的长度`vm.items.length = newLength`和利用索引直接设置一个项`vm.items[indexOfItem] = newValue`)，只能使用重写数组的7种方法来实现响应式(`push` `pop` `shift` `unshift` `splice` `sort` `reverse`）

## 7. slot是什么？有什么作用？原理是什么？
>slot是插槽，它能够作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于 <strong style="color:red">父组件 ===> 子组件</strong> -箭头传递的数据是html结构。(类似于在子组件挖个坑，等使用者使用)
>
>- 默认插槽：当slot没有命名，可以设定的默认内容，一个组件内只有有一个匿名插槽。
>
> ```vue
> 父组件中：
>         <Category>
>             //定义要传到子组件的html结构
>            <div>html结构1</div>
>         </Category>
> 子组件中(Category.vue)：
>         <template>
>             <div>
>                <!-- 定义插槽 -->
>                <slot>插槽默认内容...</slot>
>             </div>
>         </template>
> ```
>
>- 具名插槽：带有具体名字的插槽，也就是带有name属性的slot，一个组件可以出现多个具名插槽。
>
>  ```vue
>  父组件中：
>          <Category>
>              <template slot="center">  //有两种写法，一种是外面包div的slot写法;另一种是外面包template的v-slot
>                <div>html结构1</div>
>              </template>
>  
>              <template v-slot:footer>  //而不是v-slot:"footer"，v-slot只适用于最外部为template
>                 <div>html结构2</div>
>              </template>
>          </Category>
>  子组件中：
>          <template>
>              <div>
>                 <!-- 定义插槽 -->
>                 <slot name="center">插槽默认内容...</slot>
>                 <slot name="footer">插槽默认内容...</slot>
>              </div>
>          </template>
>  ```
>
>  
>
>- 作用域插槽：<span style="color:red">数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。</span>（games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定）
>
>  ```vue
>  父组件中：
>  		<Category>
>  			<template scope="scopeData">  //要用scope接收数据
>  				<!-- 生成的是ul列表 -->
>  				<ul>
>  					<li v-for="g in scopeData.games" :key="g">{{g}}</li>
>  				</ul>
>  			</template>
>  		</Category>
>  
>  		<Category>
>  			<template slot-scope="scopeData">
>  				<!-- 生成的是h4标题 -->
>  				<h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
>  			</template>
>  		</Category>
>  子组件中：
>          <template>
>              <div> 
>                  <slot :games="games"></slot>
>              </div>
>          </template>
>  		
>          <script>
>              export default {
>                  name:'Category',
>                  props:['title'],
>                  //数据在子组件自身
>                  data() {
>                      return {
>                          games:['红色警戒','穿越火线','劲舞团','超级玛丽']
>                      }
>                  },
>              }
>          </script>
>  ```
>
>  
>
>**实现原理**：当子组件vm实例化时，获取到父组件传入的slot标签的内容，存放在`vm.$slot`中，默认插槽为`vm.$slot.default`，具名插槽为`vm.$slot.xxx`，xxx 为插槽名，当组件执行渲染函数时候，遇到slot标签，使用`$slot`中的内容进行替换，此时可以为插槽传递数据，若存在数据，则可称该插槽为作用域插槽。
>
## 8. v-if和v-show的区别
>- 展示形式不同：`v-if`是条件渲染，只有当结果为true是才会创建dom节点，为false时元素节点会被销毁；`v-show`实际上是控制display属性，当结果为假时`display:none`,为真时则是`display:block`。（实际上是：v-if值为false时，在该位置创建一个注释节点，用来标识元素在页面中的位置。在值发生改变的时候，通过diff，新旧组件进行patch，从而动态显示隐藏）
>
>- 使用场景不同：
>
>	- **初次加载**：`v-if`初次加载要比`v-show`要好; `v-if`如果在初始渲染时为false，则什么也不做，直到条件第一次为true，才会开始渲染条件快；`v-show`则是不管初始条件是什么，元素总会被渲染
>
>	- **频繁切换**：`v-show`频繁切换要比`v-if`要好，创建和删除的开销比隐藏和显示的开销大
>
>- 其他：`v-if`有配套的`v-else`和`v-else-if`，而`v-show`没有；`v-if`可以搭配`template`使用，而`v-show`不行

## 9. keep-alive的理解
>keep-alive是vue系统自带的一个组件，功能是用来缓存路由组件。如果需要在组件切换的时候，保存一些组件的状态防止多次渲染，就可以使用 keep-alive 组件包裹需要保存的组件。
>
>**（1）keep-alive属性**
>
>keep-alive有以下三个属性：
>
>- include 字符串或正则表达式，只有名称匹配的组件会被匹配；
>
>- exclude 字符串或正则表达式，任何名称匹配的组件都不会被缓存；
>
>- max 数字，最多可以缓存多少组件实例
>
>```vue
><keep-alive include="News"> //这里写的名字是组件名！！是对应想要挂载的vue组件的名字
>   <router-view></router-view>
></keep-alive>
>
>//如果keep-alive没有写include默认不销毁，按多少次多少次都开启挂载
>//缓存多个路由组件，不销毁多个路由组件
><keep-alive include="['News','Message']"> 
>```
>
>**（2）使用场景**：
>
>用来缓存组件，提升项目的性能。比如实现：首页进入详情页，如果用户在首页每次点击都是相同的，那详情页就没必要请求N此，直接缓存起来就可以了，如果点击的不是同一个，那么就直接请求

## 10. 对$nextTick的理解
>1. 语法：```this.$nextTick(回调函数)```
>2. 作用：`nextTick`指定的回调会在下一次 更新DOM 结束后再执行(获取更新后的dom，把异步的推到队列里面)
>3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行
>3. 应用常将：当接收的数据是外部接口传入，可能计算不及时，需要更新dom后再同步(像异步操作)
>

## 11. Vue 中给 data 中的对象属性添加一个新的属性时会发生什么？如何解决？
>属性会添加到对象属性上，但是在页面中不会显示，因为它不是响应式的，vue2只能修改和查找对象，无法响应式地增加和删除对象。需要用到`$set()`或者`Vue.set`(删除则需要用到`$delete()`或者`Vue.delete`)

## 12. Vue中封装的数组方法有哪些，如何实现页面更新
>七种数组方法：`push` `pop` `shift` `unshift` `splice` `sort` `reverse`
>
>实现原理：重写了数组中的原生方法
>
>- 先获取原生Array的原型方法`const arrayProto = Array.prototype`
>- 对Array的原型方法使用`Object.defineProperty`做一些拦截操作(实际上是获取到数组的`__ob__`，如果有新的值就调用`observeArray`对新的值观察变化并设置响应式监听)
>- 把需要被拦截的Array类型的数据原型指向改造后的原型
>
>```js
>// 缓存数组原型
>const arrayProto = Array.prototype;
>// 实现 arrayMethods.__proto__ === Array.prototype
>export const arrayMethods = Object.create(arrayProto);
>// 需要进行功能拓展的方法
>const methodsToPatch = [
>"push",
>"pop",
>"shift",
>"unshift",
>"splice",
>"sort",
>"reverse"
>];
>
>/**
>* Intercept mutating methods and emit events
>*/
>methodsToPatch.forEach(function(method) {
> // 缓存原生数组方法
> const original = arrayProto[method];
> def(arrayMethods, method, function mutator(...args) {
>   // 执行并缓存原生数组功能
>   const result = original.apply(this, args);
>   // 响应式处理
>   const ob = this.__ob__;
>   let inserted;
>   switch (method) {
>   // push、unshift会新增索引，所以要手动observer
>     case "push":
>     case "unshift":
>       inserted = args;
>       break;
>     // splice方法，如果传入了第三个参数，也会有索引加入，也要手动observer。
>     case "splice":
>       inserted = args.slice(2);
>       break;
>   }
>   // 
>   if (inserted) ob.observeArray(inserted);// 获取插入的值，并设置响应式监听
>   // notify change
>   ob.dep.notify();// 通知依赖更新
>   // 返回原生数组方法的执行结果
>   return result;
> });
>});
>```
>
## 13. mixin
>`mixin`实际上是把多个组件共用的配置提取成一个混合对象，有全局混合和局部混合两种方法。针对data数据来说，自身组件的优先级更高，若冲突则以自身数据为准，若不存在则补充；同时可以传入生命周期，和自身组件的生命周期一起执行，但会在调用组件自身钩子之前被调用(mixin的生命周期钩子优先级更高)
>
>- 全局混合是`Vue.mixin()`
>- 局部混合是在配置接收一个数组`mixins:[]`
>
>**mixin 本质上是 mergeOptions 方法实现合并**
>
## 14. 子组件可以直接改变父组件的数据吗？
>不能。一般父子之间的传递使用props, 而props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，
>
>修改方法：
>- 若业务需求确实需要修改，可以复制props的内容到data中一份，然后去修改data中的数据
>- 可以通过 `$emit` 派发一个自定义事件，父组件接收到后，由父组件修改
>
## 15. Vue的优点
>- 数据驱动：遵循MVVM架构，使数据的更改更为简单，不需要进行逻辑代码的修改，只需要操作数据就能完成相关操作；并且有双向数据绑定，在数据操作方面更为简单
>- 组件化：实现了 `html` 的封装和重用，在构建单页面应用方面有着独特的优势
>- 操作虚拟dom: 不再使用原生的 `dom` 操作节点，极大解放 `dom` 操作，并且由于在渲染过程中，vue会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树，性能比React要快
>

## 16. $vm的实现原理
>- 如果目标是数组，直接使用数组的 splice 方法触发相应式；
>- 如果目标是对象，会先判读属性是否存在、对象是否是响应式，最终如果要对属性进行响应式处理，则是通过调用 defineReactive 方法进行响应式处理（ defineReactive 方法就是 Vue 在初始化对象时，给对象属性采用 Object.defineProperty 动态添加 getter 和 setter 的功能所调用的方法）
>
## 17. render函数原理
>实际上是为了解析引入不包含模板解析器的vue，节省打包文件体积
>他是一个函数，传入createElement，能够创建节点和节点内容
>
## 18. Vuex中action和mutation的区别
>- Action可以异步，而Mutation必须同步执行，因为Action可以和后端API结合，当vc传入的数据不确定需要访问服务器时，可以在Action执行
>- Action不能修改数据，而Mutation专门用来修改State中的数据；因为vuex的数据是集中化管理的，只有Action通过commit了数据给Mutation后才能在Mutation进行数据的更改
>- Action接收的参数是vc通过dispatch方法传递的动作类型和数据，实际上是context和value,context中存储着store的一些方法，主要是使用store中的commit；而Mutation接收的是State和传入的value
>
## 20. data和props的数据优先级
>`props>methods>data>computed>watch`  从源码体现的
>

## 21. vuex相关面试题
>### 21.1 vuex有哪些属性
>
>- State-类似于data
>- getter-类似于组件中的computed（用来提前处理State中的数据）
>- mutations-类似于组件中的methos
>- actions-提交mutations
>- modules -把以上4各属性再细分，让仓库更好管理
>

## diff算法原理

>虚拟DOM是把DOM数据化了(也就是把DOM结构封装到一个对象里面)
>
>diff算法新老节点替换的规则
>
>- 如果新老节点不是同一个节点名称，那么就暴力删除旧的节点，创建插入新的节点
>
>- 只能同级比较，不能跨层比较，如果跨层就会暴力删除旧的节点，创建插入新的节点
>
>- ps: 如果要提升性能，一定要加入key，key是唯一标志，可以在更改前后确定是不是同一节点
>
>  ```js
>  const vnode1 = h('h1',{},'你好');
>  patch(container,vnode1); //用vnode1的覆盖原本的container
>  
>  const vnode2 = h('div',{},'你好');
>  btn.onclick = function(){
>  	patch(vnode1,vnode2); 	   
>  }
>  ```
