# 基础知识

[nodejs入门](http://nodejs.cn/learn)

### Nodejs运行原理

#### Nodejs
Node是一个服务器端JavaScript解释器，用于方便地搭建响应速度快、易于扩展的网络应用。Node使用事件驱动，非阻塞I/O 模型而得以轻量和高效，非常适合在分布式设备上运行数据密集型的实时应用。
Node是一个可以让JavaScript运行在浏览器之外的平台。它实现了诸如文件系统、模块、包、操作系统 API、网络通信等Core JavaScript没有或者不完善的功能。历史上将JavaScript移植到浏览器外的计划不止一个，但Node.js 是最出色的一个。

#### V8引擎
V8 JavaScript引擎是Google用于其Chrome浏览器的底层JavaScript引擎。很少有人考虑JavaScript在客户机上实际做了些什么!

实际上，JavaScript引擎负责解释并执行代码。Google使用V8创建了一个用C++编写的超快解释器，该解释器拥有另一个独特特征；您可以下载该引擎并将其嵌入任何应用程序。V8 JavaScript引擎并不仅限于在一个浏览器中运行。

因此，Node实际上会使用Google编写的V8 JavaScript引擎，并将其重建为可在服务器上使用。

#### 事件驱动
在我们使用Java，PHP等语言实现编程的时候，我们面向对象编程是完美的编程设计，这使得他们对其他编程方法不屑一顾。却不知大名鼎鼎Node使用的却是事件驱动编程的思想。那什么是事件驱动编程。
事件驱动编程，为需要处理的事件编写相应的事件处理程序。代码在事件发生时执行。 
为需要处理的事件编写相应的事件处理程序。要理解事件驱动和程序，就需要与非事件驱动的程序进行比较。实际上，现代的程序大多是事件驱动的，比如多线程的程序，肯定是事件驱动的。早期则存在许多非事件驱动的程序，这样的程序，在需要等待某个条件触发时，会不断地检查这个条件，直到条件满足，这是很浪费cpu时间的。而事件驱动的程序，则有机会释放cpu从而进入睡眠态（注意是有机会，当然程序也可自行决定不释放cpu），当事件触发时被操作系统唤醒，这样就能更加有效地使用cpu。

#### 运行原理
当我们搜索Node.js时，夺眶而出的关键字就是 “单线程，异步I/O，事件驱动”，应用程序的请求过程可以分为俩个部分：CPU运算和I/O读写，CPU计算速度通常远高于磁盘读写速度，这就导致CPU运算已经完成，但是不得不等待磁盘I/O任务完成之后再继续接下来的业务。
所以I/O才是应用程序的瓶颈所在，在I/O密集型业务中，假设请求需要100ms来完成，其中99ms化在I/O上。如果需要优化应用程序，让他能同时处理更多的请求，我们会采用多线程，同时开启100个、1000个线程来提高我们请求处理，当然这也是一种可观的方案。
但是由于一个CPU核心在一个时刻只能做一件事情，操作系统只能通过将CPU切分为时间片的方法，让线程可以较为均匀的使用CPU资源。但操作系统在内核切换线程的同时也要切换线程的上下文，当线程数量过多时，时间将会被消耗在上下文切换中。所以在大并发时，多线程结构还是无法做到强大的伸缩性。

Node.js的单线程并不是真正的单线程，只是开启了单个线程进行业务处理（cpu的运算），同时开启了其他线程专门处理I/O。当一个指令到达主线程，主线程发现有I/O之后，直接把这个事件传给I/O线程，不会等待I/O结束后，再去处理下面的业务，而是拿到一个状态后立即往下走，这就是“单线程”、“异步I/O”。
I/O操作完之后呢？Node.js的I/O 处理完之后会有一个回调事件，这个事件会放在一个事件处理队列里头，在进程启动时node会创建一个类似于While(true)的循环，它的每一次轮询都会去查看是否有事件需要处理，是否有事件关联的回调函数需要处理，如果有就处理，然后加入下一个轮询，如果没有就退出进程，这就是所谓的“事件驱动”。这也从Node的角度解释了什么是”事件驱动”。
在node.js中，事件主要来源于网络请求，文件I/O等，根据事件的不同对观察者进行了分类，有文件I/O观察者，网络I/O观察者。事件驱动是一个典型的生产者/消费者模型，请求到达观察者那里，事件循环从观察者进行消费，主线程就可以马不停蹄的只关注业务不用再去进行I/O等待。

#### 优点
Node 公开宣称的目标是 “旨在提供一种简单的构建可伸缩网络程序的方法”。我们来看一个简单的例子，在 Java和 PHP 这类语言中，每个连接都会生成一个新线程，每个新线程可能需要 2 MB 的配套内存。在一个拥有 8 GB RAM 的系统上，理论上最大的并发连接数量是 4,000 个用户。随着您的客户群的增长，如果希望您的 Web 应用程序支持更多用户，那么，您必须添加更多服务器。所以在传统的后台开发中，整个 Web 应用程序架构（包括流量、处理器速度和内存速度）中的瓶颈是：服务器能够处理的并发连接的最大数量。这个不同的架构承载的并发数量是不一致的。
而Node的出现就是为了解决这个问题：更改连接到服务器的方式。在Node 声称它不允许使用锁，它不会直接阻塞 I/O 调用。Node在每个连接发射一个在 Node 引擎的进程中运行的事件，而不是为每个连接生成一个新的 OS 线程（并为其分配一些配套内存）。

#### 缺点
如上所述，nodejs的机制是单线程，这个线程里面，有一个事件循环机制，处理所有的请求。在事件处理过程中，它会智能地将一些涉及到IO、网络通信等耗时比较长的操作，交由worker threads去执行，执行完了再回调，这就是所谓的异步IO非阻塞吧。但是，那些非IO操作，只用CPU计算的操作，它就自己扛了，比如算什么斐波那契数列之类。它是单线程，这些自己扛的任务要一个接着一个地完成，前面那个没完成，后面的只能干等。因此，对CPU要求比较高的CPU密集型任务多的话，就有可能会造成号称高性能，适合高并发的node.js服务器反应缓慢。

1.1nodejs
nodejs组成部分：v8 engine, libuv, builtin modules, native modules以及其他辅助服务。

v8 engine：主要有两个作用 1.虚拟机的功能，执行js代码（自己的代码，第三方的代码和native modules的代码）。

　　　　　　　　　　　　   2.提供C++函数接口，为nodejs提供v8初始化，创建context，scope等。

libuv：它是基于事件驱动的异步IO模型库，我们的js代码发出请求，最终由libuv完成，而我们所设置的回调函数则是在libuv触发。

builtin modules：它是由C++代码写成各类模块，包含了crypto，zlib, file stream etc 基础功能。（v8提供了函数接口，libuv提供异步IO模型库，以及一些nodejs函数，为builtin modules提供服务）。

native modules：它是由js写成，提供我们应用程序调用的库，同时这些模块又依赖builtin modules来获取相应的服务支持

1.2your code

当我们执行node xxx.js的时候，node会先做一些v8初始化，libuv启动的工作，然后交由v8来执行native modules以及我们的js代码。

### 主要概念
词汇结构
表达式
数据类型
变量
函数
this
箭头函数
循环
作用域
数组
模板字面量
分号
严格模式
ECMAScript 6、2016、2017

异步编程与回调
定时器
Promise
Async 与 Await
闭包
事件循环

### 启动nodejs脚本
``` js
node app.js
```

### 退出程序
``` js
//强制退出 退出码(http://nodejs.cn/api/process.html#process_exit_codes)
process.exit(0)
//发送信号就进程，告诉其要发生的事情。
//SIGKILL 告诉进程要立即终止的信号
//SIGTREM 告诉进程要正常终止的信号
const express = require('express');
const app = express();
app.get('/',function(req,res){
    res.send('你好');
})
const server = app.listen(3000,()=>{console.log('服务器已就绪')})
process.on('SIGTERM',()=>{
    server.close(()=>{
        console.log('进程已终止')
    })
})

//从程序内部另一个函数中发送此信号 或者其他进程中，知道此进程pid
process.kill(process.pid,'SIGTREM');
```

### 读取环境变量
``` js
process.env.NODE_ENV //'development'
```

### 从命令行接收参数
``` js
//process.argv属性包含所有命令行调用参数的数组 第一个参数是node命令的完整路径 第二个参数是正在被执行的文件的完整路径 所有其他参数从第三个位置开始
node app.js joe //argv[0]='joe'
//或者
node app.js name=joe //argv[0]='name=joe'
//使用minimist库解析参数
node app.js --name=joe //每个参数名称之前都使用--
const args = require('minimist')(process.argv.slice(2))
args['name'] = 'joe';
```

### 输出到命令行
``` js
console.log(); //stdout
console.count(); //元素计数 
console.trace(); //打印堆栈信息
console.time() console.timeEnd(); //计算函数运行时间
console.error(); //stderr
```

### npm命令
``` js
npm install <package-name>@<version> --save || --save-dev //安装包
npm update <package-name> //更新包
npm root -g //module路径
npm init //初始化项目清单
npm list -g --depth 0 //查看所有已安装的npm包的最新版本
npm view <package-name> versions //查看包在npm中可使用的版本
npm outdated //发觉软件包的新版本
```

### package.json
``` js
{
    "name":"test", //软件包名称
    "version":"1.0.0", //当前版本
    "description":"A test project", //描述
    "main":"app.js", //入口文件
    "private":true, //如果设置为 true，则可以防止应用程序/软件包被意外地发布到 npm。
    "scripts":{ //定义了一组可以运行的 node 脚本
        "dev":"node app.js development",
        "start":"node app.js production"
    },
    "dependencies":{ // 设置了作为依赖安装的 npm 软件包的列表
        "http:":"^2.5.2"
    },
    "devDependencies":{ //设置了作为开发依赖安装的 npm 软件包的列表
        "express":"3.4.0"
    },
    "engines":{ //设置了此软件包/应用程序在哪个版本的 Node.js 上运行
        "node":">= 6.0.0",
        "npm":">= 3.0.0"
    },
    "browserslist":["> 1%", "last 2 versions", "not ie <= 8"], //用于告知要支持哪些浏览器（及其版本）
    "author":"zhang hang", //作者
    "contributors":["lisi","wangwu"], //贡献者
    "bugs":"https://github.com/nodejscn/node-api-cn/issues", //链接到软件包的问题跟踪器，最常用的是 GitHub 的 issues 页面。
    "homepage":"", //软件包主页
    "license":"MIT", //指定软件包的许可证
    "keywords":["ai","node","gateway"], //关键字
    "repository":{//此属性指定了此程序包仓库所在的位置。
        "type":"git",
        "url":"https://github.com/nodejscn/node-api-cn.git"
    }
}
```

### 事件循环
> 调用堆栈，先进后出 
``` js
const bar = ()=>console.log('bar');
const baz = ()=>console.log('baz');
const foo = ()=>{
    console.log('foo');
    //bar();
    setTimeout(bar(),0); //入队函数执行，会将函数推迟到堆栈被清空，在代码中的每个其他函数已被执行之后
    new Promise((resolve,reject)=>{
        resolve('应该在baz之后,bar之前')
    }).then(resolve=>console.log(resolve))
    baz();
}
foo();
// 执行过程
// foo()
// console.log('foo')
// setTimeout()
// new Promise()
// baz()
// resolve('...')
// bar()
// 输出结果 foo baz 'baz之后，bar之前' bar

//一个事件循环结束，下一个事件循环开始之前
process.nextTick(()=>{

})
//事件循环的下一个迭代中执行
setTimeout(()=>{},0);
setImmediate(()=>{});
```