# Deno 初体验


> 最近node.js之父又搞了个Deno.js，同样是基于V8引擎，据说对比node.js改进了不少，周末有时间就来体验了一下。

<!-- more -->

### 安装
* 安装
```js
curl -fsSL https://deno.land/x/install/install.sh | sh
```

* 编辑bash_profile文件
```js
vi ~/.bash_profile
```

* 添加环境变量
```
export DENO_INSTALL="/Users/yourName/.deno"
export PATH="$DENO_INSTALL/bin:$PATH"
```

* 执行
```
source ~/.bash_profile
```

### 体验
* 打印 Welcome to Deno
* 新建deno.ts
```js
console.log("Welcome to Deno")
```
* 运行deno run deno.ts命令
```js
deno run deno.ts
```
* 当然也可以直接运行
```js
deno run https://deno.land/std/examples/welcome.ts
```

## 支持typescript
```js
const _name: string = "jack";
const _age: number = ((): number => (Math.floor(Math.random() * 100)))()

console.log(`名字是${_name},年龄是${_age}`)
```

## 模块引入与浏览器一致
* 原先Common.js模块变为ES模块

## 增加权限
* 增加一些权限
* 比如运行一下文件需要加上--allow-net
```js
deno run --allow-net https://deno.land/std/examples/echo_server.ts
```

## 自身内置测试打包工具
* 这个就不演示了，具体看官方文档中的examples

## 结尾
**deno还有很多的特性没列出来，本文只是简单的大致体验了下deno。**

