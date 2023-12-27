# nodejs中书写自己的require方法


模块化是一种开发模式，模块化的开发有利于后期维护，提高效率，模块化的规范在服务器端是CommonJs，比如nodejs采用的就是这个，在浏览器端有AMD规范（比如RequireJs采用的），CMD规范（SeaJS采用的）。 
SeaJS我看了一点，但是我又忘得差不多了o(╥﹏╥)o，我知道错了，所以我来做笔记了。 
在SeaJs中它是用define定义模块的： 
```js
    define(function(require,exports,module) { var a=1; exports.text=a; 
    //或者
    module.exports={text:a}; 
 }) 
 ```
  三个参数： 
  require：用来加载模块的; exports：module 的别名，只能通过点语法加属性； 
  module:是一个对象，里面有exports这个属性； 
  所以exports=module.exports;
  我们在使用中用exports或者module.exports暴露接口都可以。 具体的基本使用是这样的： 
1. 引入sea.js库； 
2. 用define定义模块，在里面向外面暴露接口； 
3. 也是在define中，用require加载定义的模块； 
4. 启动模块系统，seajs.use(),引入入口模块； 有点跑题了，本篇主要是书写nodejs中的require的方法的，nodejs中的模块的定义比seajs简单点，不需要define，一个js文件就是一个模块，同样用exports暴露接口，require加载模块。 下面我们自己重写requie函数： 
```ts
 //使用严格模式 'use strict' 
 function myrequire(id) { 
    //引入node内部模块fs和path 
    const fs = require('fs'); 
    const path = require('path'); 
    //包含自身的完整路径 
    const filename = path.join(__dirname, id); 
    // pathto/module1.js 
    //不包含自身的路径 
    const dirname = path.dirname(filename); 
    // pathto 
    //为防止node把他丢到事件队列里，这里需要同步读取 
    let code = fs.readFileSync(filename, 'utf8'); 
    // 定义一个数据容器，用容器去装模块导出的成员 
    let module = { id: filename, exports: {} }; 
    let exports = module.exports; 
    // module.exports 
    code =` (function(myrequire, module, exports, __dirname, __filename) { ${code} })(myrequire, module, exports, dirname, filename);`; 
    //转换成js代码 
    eval(code); 
    return module.exports; 
} 
 ```
  再拓展解释一下上面的代码： 
1. node是只运行在V8引擎的，所以不存在兼容性问题，node在严格模式下可以用很多ES6的语法，所以你会看到let,const或是箭头函数，模板字符串的用法； 
2. node有很多内置的模块，不需要我们定义可以直接使用，比如fs,path; 
3. node的事件驱动简单的来说，先执行同步（阻塞）代码，再执行异步（非阻塞）代码。异步分为异步I/O和异步非I/O，异步非I/O就是settimeout这种，满足一定条件后才会执行，异步I/O就是fs这种读写文件的操作。
 所以它大体是这样执行代码的：先由上到下扫一遍，同步的都执行完，异步事件都扔到事件队列里，异步非I/O满足条件就执行，否则再往下走，遇到fs读写操作会交给子线程完成，自己再往下走，因为万一读的文件非常大呢？岂不耽误我主线程做事？
这种耗时的事情还是让小弟去做吧！最后子线程会以回调函数的形式返回执行结果。 其实node的require模块是有缓存机制的，当它第二次加载时会直接读取缓存里的module.exports，所以上面的方法我们还需要加入一个缓存机制，改为： 
```ts
 //使用严格模式 'use strict' 
 function myrequire(id) { 
    //引入node内部模块fs和path 
    const fs = require('fs'); 
    const path = require('path'); 
    //包含自身的完整路径 
    const filename = path.join(__dirname, id); 
    // pathto/module1.js 
    //不包含自身的路径 
    const dirname = path.dirname(filename); 
    // pathto 
    //判断是否有缓存，有的话赋值。 
    myrequire.cache = myrequire.cache || {}; 
    if (myrequire.cache[filename]) { 
        return myrequire.cache[filename].exports; 
        } 
        //为防止node把他丢到事件队列里，这里需要同步读取 
        let code = fs.readFileSync(filename, 'utf8'); 
        // 定义一个数据容器，用容器去装模块导出的成员 
        let module = { id: filename, exports: {} }; 
        let exports = module.exports; 
        module.exports code =` (function(myrequire, module, exports, __dirname, __filename) { ${code} })(myrequire, module, exports, dirname, filename);`; 
        //转换成js代码
        eval(code); 
        // 将结果缓存起来 
        myrequire.cache[filename] = module; 
        return module.exports;
    } 
 ```
  这里用到require.cache，打印它，里面是一个filename为键的对象，我们的缓存在exports中,所以每次进来只要判断一下就好了 当然你想删除require.cache里的缓存也可以，不过一般不这么做： 遍历循环里面的每一项，用delete关键字删除。 
```js
 Object.keys(require.cache).forEach(key)=>{ delete require.cache[key]; } 
 ```

