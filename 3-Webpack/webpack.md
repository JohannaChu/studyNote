# 一、介绍
## 1、简介
### （1）具体做法
将indexjs(或其中文件)中的JQ/less文件打包成chunk(代码块)
将chunk分别编译(css编译成less,js编译成jquery等)，打包作为bundle输出

### （2）模式mode
开发模式：development（能让代码本地调试运行的环境）
生产模式：production（能让代码优化上线运行的环境）:会将代码压缩

### （3）打包对象
webpack能够处理js/json资源，不能处理css/img等其他资源
生产环境和开发环境将ES6模块化编译成浏览器能识别的模块化
生产环境比开发环境多一个压缩js代码

## 2、打包-开发环境打包
主要对css,html，图片资源以及其他资源做处理
### （1）打包样式资源(css/less)
用loader：先下载再使用(配置)
css需要配置style-loader和css-loader
less需要配置style-loader,css-loader,less-loader
样式文件不会有单独输出，因为它和js的输出文件在一起
多个loader使用use,use的顺序是从下往上，从右到左

### （2）打包html资源
需要用plugins:需要下载-引入-使用
plugins会默认创建一个空的HTML，自动引入打包输出的所有资源(JS/CSS)
写的时候需要结构，需要不是空的HTML，因此写的时候要一个template，复制一个HTML
它其实算是一个构造函数，引入后需要new一下HtmlWebpackPlugin

### （3）引入图片资源
使用url-loader
url-loader不能引入在html中引入的图片(src);要用html-loader
正常引入配置url-loader即可，url-loader通常需要配置options



### （4）引入其他资源
使用file-loader
用正则表达式test查找相应格式的文件，用exclude排除相应格式的文件
exclude:/\.(css|js|html|less)$/
和处理图片资源很像，图片资源多了一个压缩功能（图片用url-loader,其他资源用file-loader）

### （5）基本配置：

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'built.js',
        path: resolve(__dirname, 'build')
},
module: {  //配置loader
    rules: [
    {
        test: /\.css$/,  //配置引入样式资源，由于是css只需引入两个即可
        use: ['style-loader', 'css-loader']  //多项引入
    },

plugins: [ //由于引入HTML而配置plugins
    new HtmlWebpackPlugin({  //相当于构造函数要new
    template: './src/index.html'  //复制文件
})],
mode: 'development'  //环境配置
};
```

### （6）devServer
用来自动化：自动编译，自动打开浏览器，自动刷新浏览器(自动编译，自动运行，自动打包)
特点：只会在内存中编译，而不会有任何输出(终端运行指令：npx webpack-dev-server)
如果使用webpack这个指令则会将打包结果输出

```
devServer: {
    contentBase:resolve(__dirname,'build'), 
    //项目构建后的路径：绝对路径并且确定输出路径
    compress:true, //启动gzip压缩
    port:3000, //确定端口号
}
```

### （7）综合配置：

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'built.js',
        path: resolve(__dirname, 'build')
},
module: {  //配置loader
    rules: [
    {   //配置引入样式资源，由于是css只需引入两个即可
        test: /\.css$/,  
        use: ['style-loader', 'css-loader']  //多项引入
    },
    {
        //处理less资源
        test: /\.less$/,  
        use: ['style-loader', 'css-loader','less-loader']
    },
    {
        //处理图片资源
        test: /\.(jpg|png|gif)$/,
        loader: 'url-loader',
        options: { //为了不让和html引入的图片资源配置相冲突(一个是commonjs)
        limit: 8*1024,
        name:'[hash:10].[ext]' //重命名，采用hash值的前10位并相同后缀值
        //关闭es6模块
        esModule: false,
        outputPath: 'img' //将输出图片放置到build/img下
    },
    {
        //处理其他资源
        exclude:/\.()html|js|css|less|png|jpg|gif/,
        loader: 'file-loader',
        options: {
            name:'[hash:10].[ext]' //重命名，采用hash值的前10位并相同后缀值
            outputPath: 'media' //将输出图片放置到build/media下
        }
    }  
    ],
    plugins: [ //由于引入HTML而配置plugins
        new HtmlWebpackPlugin({  //相当于构造函数要new
        template: './src/index.html'  //复制文件
    })
],
    mode: 'development',
    devServer: {
        contentBase:resolve(__dirname,'build'), 
        //项目构建后的路径：绝对路径并且确定输出路径
        compress:true, //启动gzip压缩
        port:3000, //确定端口号
        open: true //自动在浏览器中打开
    } 
    
    }；
```

## 3、打包-生产环境配置
### （1）区别：
将js代码从css代码中分离出来，避免出现闪屏(由style标签引入而非link标签则可能出现闪屏)
代码压缩：加载速度变快
解决兼容性问题：css兼容性处理

### （2）对样式文件处理-提取CSS成单独文件
下载插件并引入：const之后还需要new引入
（const MiniCssExtractPlugin = require('mini-css-extract-plugin')）
不使用原本的style-loader而是使用MiniCssExtractPlugin.loader
就可以将CSS的样式从JS中分离出来

### （3）CSS兼容性处理
使用postcss loader
在json中配置browserlist的条件:默认是生产环境，如果想设置开发环境则需要设置nodejs的环境
process.env.NODE_ENV = 'development';

使用loader的默认配置postcss-loader并修改配置options

### （4）压缩CSS
使用optimize-css-assets-webpack-plugin插件
步骤：
下载插件 npm i optimize-css-assets-webpack-plugin -D
引入：const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')
直接new调用：new OptimizeCssAssetsWebpackPlugin()

### （5）语法检查
语法检查只针对js，只检查自己写的源代码，不检查第三方库
用eslint-loader检查eslint，检查文件只有js文件，需要排除node_modules
要在package.json 中 eslintConfig 中设置检查规则:airbnb（需要下载三个包）

### （6）js兼容性处理
IE不认识ES6以及ES6以上的语句
用babel-loader解决，下载三个包
js兼容性处理：babel-loader @babel/core @babel/preset-env

1. 基本js兼容性处理-->@babel/preset-env
(但只能转换基本语法，比如promise高级语法就不能)

2. 全部js兼容性处理-->@babel/polyfill(直接import引入) 
但它是把所有兼容性代码都引入了，体积会增大

3. 需要做兼容性处理就做：按需加载--> core-js（需要用对象来配置）

### （7）html和js压缩
js压缩：只需要将mode调成production
html压缩：不需要对html处理兼容性，因为没有兼容性问题
在plugins里增加minify并设置配置即可

### （8）总结：生产环境配置

```
const { resolve } = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin'); //CSS压缩
const HtmlWebpackPlugin = require('html-webpack-plugin);  

// 定义 nodejs 环境变量：决定使用 browserslist 的哪个环境(css兼容性)
process.env.NODE_ENV = 'production'

// 复用 loader
const commonCssLoader = [
    MiniCssExtractPlugin.loader,
    'css-loader',
    //use数组的执行顺序是从下往上的，也就是说先执行less-loader将less转为css,然后做css的兼容性处理，最后通过css-loader将css加载到js中，再通过MiniCss提取成单独文件

    {
    // 还需要在 package.json 中定义 browserslist
        loader: 'postcss-loader',
        options: {
            ident: 'postcss',
            plugins: () => [require('postcss-preset-env')()]
        }
    }
];

module.exports = {
    entry:'./src/js/index.js',
    output: {
        filename: 'js/built.js',
        path:resolve(__dirname, 'build')
    },
    module: {
        rules: [
            {   //处理css文件并提取为单独文件
                test:/\.css$/
                use: [...commonCssLoader]
            },

            {   //处理less文件并提取为单独文件
                test:/\.less$/
                use: [...commonCssLoader,'less-loader']
            },

            {
                //js语法检查
                //在 package.json 中 eslintConfig --> airbnb
                test: /\.js$/,
                exclude: /node_modules/, //只检查自己
                // 优先执行,当一个文件要被多个loader执行时，先执行eslint再执行babel
                enforce: 'pre',
                loader: 'eslint-loader',
                options: {
                    fix: true  //自动修复问题
                }
            },

            {
                //js兼容性处理
                test: /\.js$/,
                exclude: /node_modules/, //只检查自己
                loader: 'babel-loader',
                options: {
                    presets: [ //按需兼容
                        [
                            '@babel/preset-env',
                            {
                                useBuiltIns: 'usage',
                                corejs: {version: 3},
                                targets: {
                                    chrome: '60',
                                    firefox: '50'
                                }
                            }
                        ]
                    ]
                }
            },

            {
            //处理图片资源
            test: /\.(jpg|png|gif)$/,
            loader: 'url-loader',
            options: { //为了不让和html引入的图片资源配置相冲突(一个是commonjs)
            limit: 8*1024,
            name:'[hash:10].[ext]' //重命名，采用hash值的前10位并相同后缀值
            //关闭es6模块
            esModule: false,
            outputPath: 'img' //将输出图片放置到build/img下
            },

            {
                test: /\.html$/,
                loader: 'html-loader' //引入html中的图片资源
            },

            {
            //处理其他资源
            exclude:/\.(html|js|css|less|png|jpg|gif)/,
            loader: 'file-loader',
            options: {
                name:'[hash:10].[ext]' //重命名，采用hash值的前10位并相同后缀值
                outputPath: 'media' //将输出图片放置到build/media下
                }
            }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: 'css/built.css'  //从js中提取css代码
        }),
        new OptimizeCssAssetsWebpackPlugin(), //压缩CSS
        new HtmlWebpackPlugin({  //相当于构造函数要new
            template: './src/index.html',  //复制文件
            minify: { //压缩html
                collapseWhitespace: true,
                removeComments: true
            }
    ]
    mode: 'production'  //js压缩
};
```

# 二、优化配置
## 1、webpack性能优化
### （1）开发环境性能优化
优化打包构建速度：HMR
优化代码调试:source-map

### （2）生产环境性能优化
优化打包构建速度:

1. oneOf
2. babel缓存
3. 多进程打包

优化代码运行的性能:
1. 缓存(hash-chunkhash-contenthash)
2. tree shaking
3. code split
4. 懒加载&预加载
5. PWA
6. externals
7. dll

## 2、具体做法
### （1）HMR功能
hot module replacement:热模块替换/模块热替换
作用：一个模块发生改变，只会重新打包这一个模块(而不是打包所有模块)，可以极大提升构建速度-->css可以实现，js要自己写，HTML不需要
使用方法：在devServer里面写hot:true即可

样式文件：可以使用HMR功能，因为style-loader内部可以实现
js文件：默认不能使用HMR功能-->修改js代码，添加支持HMR功能的代码
HTML文件：默认不能使用HMR功能，同时会导致问题：HTML文件不能热更新(代码更新没有效果)
但是HTML文件不需要做HMR功能

### （2）source-map
是一种 提供源代码到构建后代码映射 技术（如果构建后代码出错了可以通过映射追踪源代码错误）

[inline-|hidden-|eval-][nosources-][cheap-[module]]source-map 可以组合

1. source-map:外部(提供错误代码准确信息和源代码的错误位置)
2. inline-source-map:内联(只生成一个内联source-map,提供错误代码准确信息和源代码的错误位置)
3. hidden-source-map:外部(错误代码错误原因，但没有错误位置,不能追踪源代码错误，只能提示到构建后代码的错误位置)
4. eval-source-map:内联(每个文件都生成对应的source-map,都在eval，提供错误代码准确信息和源代码的错误位置)
5. nosources-source-map:外部(提供错误代码准确信息，但没有任何源代码信息)
6. cheap-source-map:外部(错误代码准确信息和源代码错误位置，但只能精确到行不能到列)
7. cheap-module-source-map:外部(错误代码准确信息和源代码错误位置)

内联和外部的区别：
1. 外部生成了文件，内联没有
2. 内联构建速度更快

开发环境：速度快，调试更友好
1. 速度快(eval>inline>cheap>...)
可以使用eval-cheap-source-map或者eval-source-map

2. 调试更友好
可以使用source-map或者cheap-module-source-map或者cheap-source-map

综合：开发环境可以使用eval-source-map或者eval-cheap-module-source-map

生产环境：源代码需不需要隐藏？调试要不要更友好？
内联会让代码体积变大，因此在生产环境不用内联
可以用nosources-source-map全部隐藏
或者用hidden-source-map只隐藏源代码，会提示构建后代码错误信息

综合：生产环境可以使用source-map或者cheap-module-source-map

### （3）oneOf
作用：loader只会匹配一个
但是注意不能有两个配置处理同一类文件例如eslint和babel-loader

### （4）缓存
由于大部分文件都是js文件，在运行时会耗费大量时间，因此可以提前缓存js文件
(主要用eslint:检查和babel:转换)

1. babel缓存：第二次构建时，会读取之前的缓存--> 让第二次打包构建速度更快
cacheDirectory:true

2. 文件资源缓存：让代码上线运行缓存更好使用
hash:每次webpack构建时会生成一个唯一的hash值，即使文件没有变，每一次重新打包hash值也会改变
问题：因为js和css同时使用一个hash值，如果重新打包则会导致所有缓存失效

chunkhash:根据chunk生成的hash值；如果打包来源于同一个chunk,则hash值相同
问题：js和css的hash值还是一样，因为css是在js中被引入的，所以属于同一个chunk

contenthash：根据文件的内容生成hash值，不同文件hash值一定不一样

ps:使用hash值时要使用runtime方式将单独的一个文件的hash值打包，否则会导致a文件的a.js文件修改导致b文件的b.js文件也被修改

### （5）tree shaking
去除无用代码，可以减少代码体积
前提：1.必须使用ES6模块化  2. 开启production环境

需要在package.json中设置
若设置为"sideEffects"则代表所有代码都没有副作用(可能会把css/@babel/polyfill等文件都去除)
因此需要设置为"sideEffects:["*.css"]"

### （6）代码分割 code split
将一个大的js文件设置为多个小的js文件，有单入口拆分和多入口拆分两种方式

1. 在entry设置多入口
2. 使用optimization: {splitChunks: chunks:'all'}
- 可以将node_modules中代码单独打包一个chunk的最终输出(单入口拆分)
- 多入口拆分：可以自动分析多入口chunk中有没有公共的文件，若有则会打包成一个单独的chunk(例如jquery)
3. 通过js代码，让某个文件被单独打包成一个chunk：通过import动态引入语法

### （7）js代码懒加载&预加载
利用前面代码分割的思想，将import放入一个异步函数中
懒加载：当文件需要时才加载，但当文件较大时会造成点击延迟
Prefetch预加载：会在使用之前就提前加载js文件，但是有兼容性问题，在移动端不能使用
正常加载可以认为是并行加载(同一时间加载多个文件)
预加载是等其他资源加载完毕，等浏览器空闲再偷偷加载资源

例如：
```
document.getElementById('btn').onclick = function() {
    //懒加载
    import(/*webpackChunkName: 'test',webpackPrefetch:true */'./test').then(({mul})) => { 
        console.log(mul(4,5));
    }
}
```

### （8）PWA：离线也可以访问
PWA: 渐进式网络开发应用程序(离线可访问)
使用workbox包 --> workbox-webpack-plugin

```
new WorkboxpackPlugin.GenrateSW({
    //帮助serviceworker快速启动以及删除旧的serviceworker

    //生成一个serviceworker配置文件
    clientsClaim: true, 
    skipWaiting: true
})
```
在入口文件中注册serviceworker并处理兼容性问题

### （9）多进程打包
下载thread-loader包，并对babel-loader使用
但是适用于多js代码的情况，因为进程启动大概为600ms,进程通信也需时间
只有工作消耗时间较长才需要多进程打包

### （10）external
拒绝某一指定的库被打包，因此自行在需要在入口文件引入

```
externals: {
    jquery: 'jQuery' //后面是库名
}
```

### （10）dll
作用：将不同的库打包为不同的chunk（对某些如jquery库进行单独的打包）

1. 定义一个webpack.dll.js并运行(作用是单独打包jquery库并且生成manifest.json提供和jQuery的映射关系)
2. 通过引入webpack插件告诉webpack哪些库不参与打包，使用时名称也得改变（通过manifest忽略jquery）
3. 通过AddAssetHtmlWebpackPlugin将某个文件打包输出去，并在html中自动引入该资源(引入jquery)

externals和dll都是不打包某个库，但是external是直接不打包，需要时在入口文件自行引入；dll是先把某个包单独打包好，引入时通过插件排除这些库然后再自动引入(这样以后就不需要再引入)

# 三、webpack详细配置
## 1、entry
### （1）string
单入口--> entry:'./src/index.js'-->打包形成一个chunk,输出一个bundle文件
有常见的entry写法和filename为'[name].js'的写法(默认命名为main.js)

### （2）array
多入口 --> entry:['./src/index.js', './src/add.js']
所有入口文件最终只会形成一个chunk,输出只有一个bundle文件
作用：在HMR功能中让html热更新生效

### （3）object
多入口--> entry:{index: './src/index.js', add: './src/add.js'}
有几个入口文件就形成几个chunk,输出几个bundle,此时chunk的名称是key(例如index&add)

## 2、output

```
output: {
    //文件名称(指定名称+目录)
    filename: 'js/[name].js',
    //输出文件目录（将来所有资源输出的公共目录）
    path:resolve(__dirname, 'build'),
    //所有资源引入公共路径前缀 --> 例如：'imgs/a.jpg' --> '/imgs/a.jpg'，一般用于生产环境
    publicPath: '/',
    //非入口chunk名称，针对import和optimization引入的chunk
    chunkFilename:'js/[name]_chunk.js'
    library: '[name]', // 整个库向外暴露的变量名
    libraryTarget: 'window' // 变量名添加到哪个上 这里指的是添加到browser
    libraryTarget: 'global' // 变量名添加到哪个上 这里指的是添加到node
    libraryTarget: 'commonjs'  //通过commonjs暴露
    library一般与dll一起使用
}
```

## 3、module

```
module: {
    rules: [
    // loader 的配置
    {
        test: /\.css$/,
        // 多个 loader 用 use,单个loader直接loader:即可
        use: ['style-loader', 'css-loader']
    },
    {
        test: /\.js$/,
        // 排除 node_modules 下的 js 文件
        exclude: /node_modules/,
        // 只检查 src 下的 js 文件
        include: resolve(__dirname, 'src'),
        // 优先执行
        enforce: 'pre',
        // 延后执行
        enforce: 'post',
        // 单个 loader 用 loader
        loader: 'eslint-loader',
        options: {} //通过配置指定
    },
    {
    // 以下配置只会生效一个
    oneOf: []
    }
```

## 4、resolve

```
// 解析模块的规则
resolve: {
    // 配置解析模块路径别名: 优点简写路径 缺点路径没有提示
    alias: {
    $css: resolve(__dirname, 'src/css')
    },
    // 配置省略文件路径的后缀名,文件名不要写成一样的
    extensions: ['.js', '.json', '.jsx', '.css'],
    // 告诉 webpack 解析模块是去找哪个目录
    modules: [resolve(__dirname, '../../node_modules'), 'node_modules']
}   
```

## 5、devServer

```
devServer: {
    // 运行代码的目录
    contentBase: resolve(__dirname, 'build'),
    // 监视 contentBase 目录下的所有文件，一旦文件变化就会 reload
    watchContentBase: true,
    watchOptions: {
    // 忽略文件
        ignored: /node_modules/
    },
    // 启动 gzip 压缩
    compress: true,
    // 端口号
    port: 5000,
    // 域名
    host: 'localhost',
    // 自动打开浏览器
    open: true,
    // 开启 HMR 功能
    hot: true,
    // 不要显示启动服务器日志信息
    clientLogLevel: 'none',
    // 除了一些基本启动信息以外，其他内容都不要显示
    quiet: true,
    // 如果出错了，不要全屏提示~
    overlay: false,
    // 服务器代理 --> 解决开发环境跨域问题
    proxy: {
    // 一旦 devServer(5000)服务器接受到 /api/xxx 的请求，就会把请求转发到另外一个服务器(3000)
    '/api': {
        target: 'http://localhost:3000',
        // 发送请求时，请求路径重写：将 /api/xxx --> /xxx （去掉/api）
        pathRewrite: {
            '^/api': ''
            }
        }
    }
}
};
```

## 6、optimization
当a文件引入b文件，一旦b文件的hash值改变，a文件也会改变，runtimeChunk就是把hash值单独打包为一个chunk;即使b文件改变，也是hash-chunk改变，a文件不会变

```
optimization: { //缓存默认存在问题，加上runtimeChunk可以解决问题
    splitChunks: {
        chunks: 'all'  //默认值，可以不写~
    },

    // 将当前模块的记录其他模块的 hash 单独打包为一个文件 runtime
    // 解决：修改 a 文件导致 b 文件的 contenthash 变化
    runtimeChunk: {
        name: entrypoint => `runtime-${entrypoint.name}`
    },

    // 可以修改生产环境的压缩方案：js 和 css
    minimizer: [
        new TerserWebpackPlugin({
        // 开启缓存
        cache: true,
        // 开启多进程打包
        parallel: true,
        // 启动 source-map
        sourceMap: true
    })
    ]
}
```

# 四、webpack5
主要是tree shaking有了显著提升，压缩体积显著性减小
此版本重点关注以下内容:

- 通过持久缓存提高构建性能.
- 使用更好的算法和默认值来改善长期缓存.
- 通过更好的树摇和代码生成来改善捆绑包大小.
- 清除处于怪异状态的内部结构，同时在 v4 中实现功能而不引入任何重大更改.
- 通过引入重大更改来为将来的功能做准备，以使我们能够尽可能长时间地使用 v5.

## （1）下载

- npm i webpack@next webpack-cli -D

## （2）自动删除 Node.js Polyfills

早期，webpack 的目标是允许在浏览器中运行大多数 node.js 模块，但是模块格局发生了变化，许多模块用途现在主要是为前端目的而编写的。webpack <= 4 附带了许多 node.js 核心模块的 polyfill，一旦模块使用任何核心模块（即 crypto 模块），这些模块就会自动应用。

尽管这使使用为 node.js 编写的模块变得容易，但它会将这些巨大的 polyfill 添加到包中。在许多情况下，这些 polyfill 是不必要的。

webpack 5 会自动停止填充这些核心模块，并专注于与前端兼容的模块。

迁移：

- 尽可能尝试使用与前端兼容的模块。
- 可以为 node.js 核心模块手动添加一个 polyfill。错误消息将提示如何实现该目标。

## （3）Chunk 和模块 ID

添加了用于长期缓存的新算法。在生产模式下默认情况下启用这些功能。

`chunkIds: "deterministic", moduleIds: "deterministic"`

## （4）Chunk ID

你可以不用使用 `import(/* webpackChunkName: "name" */ "module")` 在开发环境来为 chunk 命名，生产环境还是有必要的

webpack 内部有 chunk 命名规则，不再是以 id(0, 1, 2)命名了

## （5）Tree Shaking

1. webpack 现在能够处理对嵌套模块的 tree shaking

```js
// inner.js
export const a = 1;
export const b = 2;

// module.js
import * as inner from './inner';
export { inner };

// user.js
import * as module from './module';
console.log(module.inner.a);
```

在生产环境中, inner 模块暴露的 `b` 会被删除

2. webpack 现在能够多个模块之前的关系

```js
import { something } from './something';

function usingSomething() {
  return something;
}

export function test() {
  return usingSomething();
}
```

当设置了`"sideEffects": false`时，一旦发现`test`方法没有使用，不但删除`test`，还会删除`"./something"`

3. webpack 现在能处理对 Commonjs 的 tree shaking

## （6）Output

webpack 4 默认只能输出 ES5 代码

webpack 5 开始新增一个属性 output.ecmaVersion, 可以生成 ES5 和 ES6 / ES2015 代码.

如：`output.ecmaVersion: 2015`

## （7）SplitChunk

```js
// webpack4
minSize: 30000;
```

```js
// webpack5
minSize: {
  javascript: 30000,
  style: 50000,
}
```

## （8）Caching

```js
// 配置缓存
cache: {
  // 磁盘存储
  type: "filesystem",
  buildDependencies: {
    // 当配置修改时，缓存失效
    config: [__filename]
  }
}
```

缓存将存储到 `node_modules/.cache/webpack`

## （9）监视输出文件

之前 webpack 总是在第一次构建时输出全部文件，但是监视重新构建时会只更新修改的文件。

此次更新在第一次构建时会找到输出文件看是否有变化，从而决定要不要输出全部文件。

## （10）默认值

- `entry: "./src/index.js`
- `output.path: path.resolve(__dirname, "dist")`
- `output.filename: "[name].js"`
