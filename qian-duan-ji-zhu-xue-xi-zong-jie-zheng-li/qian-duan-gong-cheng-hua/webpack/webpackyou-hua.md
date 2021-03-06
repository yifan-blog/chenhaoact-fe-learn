# webpack性能优化

**搜索引擎搜索 webpack优化 就会有很多不错的文章，任何技术都应该多搜一搜看一看 ...优化 方面的文章。**

如果引用了ant-d:

ant-d按需加载\(使用babel-plugin-import 是一个用于按需加载组件代码和样式的 babel 插件：

[https://ant.design/docs/react/use-with-create-react-app-cn](https://ant.design/docs/react/use-with-create-react-app-cn) 参考高级配置和按需加载部分

实践后，app.js从 14.8MB 降到 4.79MB! 看来正确的按需加载和引用ant-d等组件库对性能很重要。

基于 Webpack 的应用包体尺寸优化

[https://zhuanlan.zhihu.com/p/24928279](https://zhuanlan.zhihu.com/p/24928279)

简单几步助你优化React应用包体

[https://segmentfault.com/a/1190000007692543](https://segmentfault.com/a/1190000007692543)

开发工具心得：如何 10 倍提高你的 Webpack 构建效率

[https://segmentfault.com/a/1190000005770042](https://segmentfault.com/a/1190000005770042)

webpack使用优化

[http://www.tuicool.com/articles/bAzMju](http://www.tuicool.com/articles/bAzMju)

[http://www.alloyteam.com/2016/01/webpack-use-optimization/](http://www.alloyteam.com/2016/01/webpack-use-optimization/)

彻底解决Webpack打包性能问题\(使用**webpack.DllPlugin和webpack.DllReferencePlugin可以加速打包效率，提升打包速度，在依赖的库较多的情况下很有效，需要配置ddl.config.js，动态链接库（dll）**\)

[https://zhuanlan.zhihu.com/p/21748318](https://zhuanlan.zhihu.com/p/21748318)

[前端性能优化：webpack分离 + LocalStorage缓存](http://blog.csdn.net/code_for_free/article/details/52556896)

## 

**webpack2相关优化：**

[webpack代码分离按需加载](https://doc.webpack-china.org/guides/code-splitting/)（webpack2官方教程中的，重要！）

[webpack2 终极优化](/h ttp://www.open-open.com/lib/view/open1483317889255.html)

## webpack及reacct优化常用方法整理及实践

### 1.**各个模块，组件库，类库，按需引入，按需加载。**

引入某个功能的时候，应该只引入需要的模块，以 Lodash 为例：

```
//1.Import the entire lodash library and add it to the bundle

import lodash from 'lodash'

lodash.groupBy(rows, 
'id'
)

//2.Import only the required function from lodash

import groupBy from 'lodash/groupBy'

groupBy(rows, 
'id'
)
```

再如：ant-d按需加载\(使用babel-plugin-import 是一个用于按需加载组件代码和样式的 babel 插件：[https://ant.design/docs/react/use-with-create-react-app-cn](https://ant.design/docs/react/use-with-create-react-app-cn) 参考高级配置和按需加载部分

实践后，app.js从 14.8MB 降到 4.79MB,vendors.js为1.25MB! 看来正确的按需加载和引用ant-d等组件库对性能很重要。

### （2）使用一些webpack插件减小打包文件大小

提升性能基于 Webpack 的应用包体尺寸优化[https://zhuanlan.zhihu.com/p/24928279和简单几步助你优化React应用包体https://segmentfault.com/a/1190000007692543](https://zhuanlan.zhihu.com/p/24928279和简单几步助你优化React应用包体https://segmentfault.com/a/1190000007692543)

实践后，文件进一步减小，

其中：

在生产环境中将NODE\_ENV设置为productionapp.js从4.79MB降到4.63MB,vendors.js为1.25MB

使用new webpack.optimize.DedupePlugin\(\)后app.js从降到4.51MB,vendors.js为1.25MB

使用new webpack.IgnorePlugin\(/^.\/locale$/, \[/moment$/\]\), 用于忽略引入模块中并不需要的内容，减少打包文件大小，提升性能。譬如当我们引入moment.js时，我们并不需要引入该库中所有的区域设置，因此可以利用该插件忽略不必要的代码。app.js为4.51MB,而vendors.js则由1.25MB大幅度降到329kB（看来原来的vendors.js中moment不需要的部分就占了一大部分）

### （3）代码压缩（比如webpack打包gzip压缩文件并在服务端优先加载）

使用compression-webpack-plugin插件后,将资源文件压缩为.gz文件，并且根据客户端的需求按需加载build目录多出来了app.js.gz（1.09 MB）和vendors.js.gz（80.1 kB），可以看出，使用gz压缩后的文件比原文件小了很多由于我的服务端使用的是koa的koa-static（[https://github.com/koajs/static）来加载静态资源，gzip](https://github.com/koajs/static）来加载静态资源，gzip) Try to serve the gzipped version of a file automatically when gzip is supported by a client and if the requested file with .gz extension exists. defaults to true.所以如果有gzip的压缩文件会默认去解析，从而提升应用的性能。

经过这些步骤，部署在云服务器node应用后，加载所有前端资源的时间有一开始的近1.8min,变为了9.7s,很大的提升了性能和体验\(当然离2秒以内加载完的体验要求相差还有些远，后面还会有其他的优化措施，本地开发时启的koa服务加载只需要2.6s左右，也能够看出本地环境和真实的服务器端有着不少的差距\)：

优化前：![](/assets/importwbpyhq.png)

优化后：![](/assets/importyhh.png)

### \(4\)将css和js分开打包和加载

### 使用插件extract-text-webpack-plugin，将css单独打包，不打入js,提升页面加载速度和性能，参考插件使用：[https://github.com/webpack-contrib/extract-text-webpack-plugin](https://github.com/webpack-contrib/extract-text-webpack-plugin)

首页里先加载css

实践后，app.js.gz（1.09 MB-&gt;1.03MB）和vendors.js.gz（80.1 KB-&gt;80.1KB）,多出style.css.gz\(13.6KB\)，可以看出少于app.js.gz减少的大小，有效的进行了一定的性能优化：

![](/assets/importyhhhhh.png)

 部署在云服务器node应用后，加载所有前端资源的时间由9.7s减少为8.13s:![](/assets/importyhhhhhhhhhh.png)

### （5）统一提取库代码

统一把需要用到的一些库，单独提出来到venders比较好，不要和自己的业务代码混在一起，原来只有moment放在vendors下（但是注意没有用到的库不要写比如lodash这里还没有用到，如果写了没用到的库反而会使vendors.js变大）

`entry: {`

`app: path.resolve(APP_PATH, 'index.jsx'),`

`//提取第三方库，单独打包到venders.js,提升性能`

`vendors: ['react', 'react-dom', 'react-router', 'react-redux', 'moment'] //需要用到的一些库，最好单独提出来到venders比较好，不要和自己的业务代码混在一起`

`}`

实践后，app.js.gz（1.03 MB-&gt;382KB）和vendors.js.gz（80.1 KB-&gt;728KB），两者仍然和原来相同：

![](/assets/importdxdx.png)

可以看到两个js的大小均衡了一些，原来则是app.js比vendors.js大很多，现在业务代码app.js里的类库代码减少，这样更合理一些。![](/assets/importksks.png)

### （6）js代码拆分（代码分割），按需加载

参考：

webpack代码分割技巧

[http://www.tuicool.com/articles/UZRF3qM](http://www.tuicool.com/articles/UZRF3qM)

### （7）升级到webpack2

详细参考我的这篇文章：

webpack从1到2的升级和对比

升级后的速度：

升级webpack2后又参考以下内容做了优化：

开发工具心得：如何 10 倍提高你的 Webpack 构建效率

[https://segmentfault.com/a/1190000005770042](https://segmentfault.com/a/1190000005770042)

webpack使用优化

[http://www.tuicool.com/articles/bAzMju](http://www.tuicool.com/articles/bAzMju)

[http://www.alloyteam.com/2016/01/webpack-use-optimization/](http://www.alloyteam.com/2016/01/webpack-use-optimization/)

webpack2 终极优化

[http://www.open-open.com/lib/view/open1483317889255.html](http://www.open-open.com/lib/view/open1483317889255.html)

彻底解决Webpack打包性能问题\(使用**webpack.DllPlugin和webpack.DllReferencePlugin可以加速打包效率，提升打包速度，在依赖的库较多的情况下很有效，需要配置ddl.config.js，动态链接库（dll）**\)

[https://zhuanlan.zhihu.com/p/21748318](https://zhuanlan.zhihu.com/p/21748318)

[前端性能优化：webpack分离 + LocalStorage缓存](http://blog.csdn.net/code_for_free/article/details/52556896)

## 

后期会尝试从下列几个方向去优化：

1. 将js分离，不同页面加载不同的js；
2. 将React剥离出去，使用cdn;
3. 使用React，可以尝试后端渲染；
4. 设置缓存


## 其他参考

webpack 构建性能优化策略小结
https://segmentfault.com/a/1190000007891318

