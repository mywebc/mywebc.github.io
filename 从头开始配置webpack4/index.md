# 从头开始配置webpack4

>最近好像流行零配置，parcel的开箱即用的概念，也影响了webpack4,在 
webapck4中也开始朝着这个方向发展，比如默认的入口为src,默认的打包输出为dist,新增了mode属性，有利于拆分生产环境和开发环境，默认为production，并且默认压缩js，还有一些其他的插件也不再适用，在此也没必要了解，最近就从头梳理了一下webpack4.

<!-- more -->
> 本片文章主要参考自董沅鑫的个人博客[https://godbmw.com](https://godbmw.com)

## 环境
首先我们要全局安装webpack-cli和webpack,然后也要在项目中安装webpack和webpack-cli,这样我们才能使用。
## 编译ES6
编译es6,我们需要以下loaders
* babel-loader  **babel转换器**
* babel-core    **babel转换时所调用的api(听名字就知道它是babel的核心)**
* babel-preset-env **语法转换规则**
* babel-plugin-transform-runtime和Babel-runtime **polyfill按需引入**
* babel-polyfill **polyfill全局引入**
**注意：在babel7中所有前缀都变为@babel/,注意版本的统一**

新建webpack.config.js:
```javascript
const path = require("path")
// 使用babel编译ES6
module.exports = {
    entry: {
        app: "./index.js" // 以对象形式写推荐
    },
    output: {
        filename: "bundle.js",
        // resolve解析为一个绝对路径
        path: path.resolve(__dirname, "dist")
    },
    module: {
        rules: [
            {
                test: /\.js$/, // 正则匹配以js结尾的文件
                use: {
                    loader: "babel-loader"
                    // option: { // babel-loader的具体配置信息写在根目录下的.babelrc中
                    // }
                },
                exclude: /node_modules/ // 排除依赖包
            }
        ]
    }
}
```
babel的配置一般放在.babelrc中
```javascript
{
    "presets": [
        "@babel/env",{
            "targets": {
               "browsers": ["last 2 versions"] 
            }
        }
    ],
    "plugins": ["@babel/plugin-transform-runtime"]
}
```
* presets配置中你可以具体指定，比如这里指定为浏览器上两个版本

* 我们知道用babel来转换es6语法，语法规则根据presets（预设的规则来的），**但却转换不了es6的一些方法比如promise，generator函数，所以我们需要polyfill(垫片)**
* 上面用的是按需引入polyfill,@babel/plugin-transform-runtime这个插件会去@babel/runtime中自动引入，**所以即使打包后我们也用到@babel/runtime，它应该安装在生产环境中的依赖包中**
* 如果要使用babel-polyfill的话，安装@babel/polyfill,并且在webpack.config.js中最开头引入即可，或者在写在入口函数字符串数组第一个
```javascript
import '@babel/polyfill'
// 或者
module.exports = {
entry: ["@babel/polyfill","index.js"]
}
```

## 提取公共JS
* 在配置中新增optimization属性
```javascript
const path = require("path")
// 多页面打包，提取公共代码
module.exports = {
    entry: {
        a: './a.js',
        b: './b.js'
    },
    output: {
        filename: '[name].bundle.js',
        chunkFilename: "[name].chunk.js", // 非入口打包文件
        path: path.resolve(__dirname, 'dist')
    },
    optimization: {
        splitChunks: {
          cacheGroups: {
            commons: {
                name: 'common', // 提取公共chunk的名字
                priority: 0, // 缓存优先级
                chunks: 'initial', // 作用范围
                minChunks: 2,//最小引用2次
                minSize: 0 // 只要超出0字节就生成一个新包
              },
              vendor: {
                name: "vendor",
                test: /node_modules/,
                chunks: "initial",
                priority: 10,
                chunks: 'all'
              }
          }
        }
      }
}
```
* 这里最重要的是priority属性，表示优先级的意思，上面我们就是先打包node_modules,打包后的名字为vendor,其次再打包Js,取名为common，
* 输出的时候也要注意加上ChunkFilename,表示非入口打包文件，抽离出的公共部分就是非入口打包文件
* 具体的其他属性，可以去官网去看看，这里只给出基本示例

## 代码分割与懒加载
在入口文件中使用import()或者require.ensure()方法，webpack.config.js并不需要配置
示例
```javascript
import(/* webpackChunkName: 'a'*/ "./a").then(function(a) {
    console.log(a);
  });
import(/* webpackChunkName: 'b'*/ "b").then(function(b) {
    console.log(b));
  });
// 或者
require.ensure(
  ["./a.js", "./b.js"],
  function () {
    var a = require("./a");
    var b = require("./b");
    console.log("加载完成")
  },
  "page"
);
```
* 我们在vue中使用路由懒加载就是使用import()
* 我们在node中因为CommonJs规范为同步加载模块，我们使用的是require.ensure()来按需加载，其实这些方法我们以前就用过

## 处理CSS
### 解析CSS
需要用到的loaders
* style-loader
* css-loader
* sass-loader/node-sass(以sass为例)
```javascript
const path = require("path")
module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        filename: "[name].bundle.js",
        path: path.resolve(__dirname, "dist")
    },
    module: {
        rules: [
            {
                test:/\.scss$/,
                use: ["style-loader","css-loader","sass-loader"]// 如果每个loader有配置，可以以对象形式书写
            }
        ]
    }
}
```
### 提取CSS
需要用到的loaders
* extract-text-webpack-plugin@next（注意后缀@next意为最新版）
* 或者mini-css-extract-plugin
```javascript
const path = require("path")
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
    entry: {
        app: './src/app.js'
    },
    output: {
        filename: "[name].bundle.js", // []表示占位符
        path: path.resolve(__dirname, "dist")
    },
    module: {
        rules: [
            {
                test: /\.scss$/,
                use: ExtractTextPlugin.extract({
                    fallback: {
                        loader: "style-loader" // css编译后用什么来提取的loader
                    },
                    use: [
                        {
                            loader: "css-loader",
                            options: {
                                minimize: true // 是否压缩
                            }
                        },
                        {
                            loader: "sass-loader"
                        }
                    ]
                })
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin("[name].min.css")
        // new ExtractTextPlugin({
        //     filename: "[name].min.css", // 压缩后的名字
        // })
    ]
}
```
## 处理图片
* 通过CSS引入图片
style-loader和css-loder解析CSS,用url-loader解析图片
* html标签插入图片（webpack不推荐这么引用） **html-withimg-loader**
```javascript
const path = require("path")
const webpack = require("webpack")
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
    entry: {
        app: "./src/app.js"
    },
    output: {
        filename: "app.[hash:8].bundle.js",
        path: path.resolve(__dirname, "dist"),
        chunkFilename: "[name].chunk.js",
        publicPath: "./", // 资源引用路径
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ExtractTextPlugin.extract({
                fallback: {
                    loader: "style-loader", // 回调style注入
                    options: {
                    singleton: true
                    }
                },
                use: {
                    loader: "css-loader",// 解析url()和@import
                    options: {
                    minimize: true
                    }
                }
                }) 
            },
            {
                test: /\.(png|jpg|jpeg|gif)$/,
                use: [
                    {
                    loader: "url-loader", // 解析图片
                    options: {
                        name: "[name]-[hash:5].min.[ext]",
                        limit: 10000, // size <= 20KB
                        publicPath: "static/",
                        outputPath: "static/"
                    }
                    }
                ]
            },
            // 也可以对字体处理
            {
                test: /\.(eot|woff2?|ttf|svg)$/,
                use: [
                  {
                    loader: "url-loader",
                    options: {
                      name: "[name]-[hash:5].min.[ext]",
                      limit: 5000, // fonts file size <= 5KB, use 'base64'; else, output svg file
                      publicPath: "fonts/",
                      outputPath: "fonts/"
                    }
                  }
                ]
              },
              // html标签方式插入图片
              {    test: /\.(htm|html)$/i,
                use:[ 'html-withimg-loader'] 
           }
        ]
    },
    plugins: [
        // 压缩后CSS的名字
        new ExtractTextPlugin("app.[hash:8].min.css"),
    ],
}
```


## tree shaking
* js tree shaking
```javascript
module.exports = {
    // 设置为生产环境，自动调用插件
    mode: "production"
}
```
* css tree shaking
需要用到的loaders
* purifycss-webpack **shaking主要插件**
* glob-all **帮助指示路径**
* extract-text-webpack-plugin **提取分离CSS**
```javascript
const path = require("path")
const PurifyCSS = require("purifycss-webpack");
const glob = require("glob-all");
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
    entry: {
        app: "./src/app.js"
    },
    output: {
        filename: "app.[hash:8].bundle.js",
        path: path.resolve(__dirname, "dist"),
        chunkFilename: "[name].chunk.js",
        publicPath: __dirname + "/dist/",
    },
    module: {
        rules: [
          {
            test: /\.css$/,
            use: ExtractTextPlugin.extract({
              fallback: {
                loader: "style-loader", // 编译后使用style插入
                options: {
                  singleton: true
                }
              },
              use: {
                loader: "css-loader",
                options: {
                  minimize: true
                }
              }
            })
          }
        ]
      },
    plugins: [
        new ExtractTextPlugin("app.[hash:8].min.css"),
        new PurifyCSS({
          paths: glob.sync([
            path.join(__dirname, ".html"),
            path.join(__dirname, "src/*.js")
          ])
        }),
    ],
}
}
```
## 自动生成html&&清除打包缓存
```javascript
const CleanWebpackPlugin = require('clean-webpack-plugin');
const htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    plugins: [
        // 字符串数组
        new CleanWebpackPlugin(['dist']),
        // 生成html,并且自动引入
        new htmlWebpackPlugin({
            filename: "index.html",  //打包后的文件名
            template: path.join(__dirname , "./index.html")  // 可以指定模板，如果是自定义的模板需要下载对于loader
        }),
    ],
}
```

## 第三方js库
jquery为例
npm install jquery --save
npm i expose-loader --save
```javascript

module.exports = {
   module: {
       {
        test: require.resolve('jquery'),
        use: [{
            loader: 'expose-loader',
            options: 'jQuery'
        },{
            loader: 'expose-loader',
            options: '$'
        }]
    }
   }
}
```
然后在组件中可以这样引用
```javascript
require('jquery')
require('jQuery第三方插件')
```

## webpack-dev-server
```javascript
module.exports = {
    devServer: {
        contentBase: path.join(__dirname, 'dist'),  //启动路径
        host:'localhost',  //域名
        port: 8888,  //端口号
         // 声明为热替换
        hot: true,
        // 第一次打包时打开浏览器
        open: true,
        compress:true, //压缩,
        overlay: true // 浏览器显示错误
        //请求到 /api/users 现在会被代理到请求 http://localhost:9000/api/users。
        // proxy: {
        //     "/api": "http://localhost:9000",
        // }
    }
}
```
## 生产环境和开发环境
我们可以把我们的webpack.config.js复制两份,使用mode属性来拆分成如下
webapck.dev.config.js **mode:"development"**
webapck.pro.config.js **mode:"production"**
然后在package.json的书写脚本
```javascript
{
"dev": "webpack --config webpack.dev.config.js"
"build": "webpack --config webpack.pro.config.js"
}
```


