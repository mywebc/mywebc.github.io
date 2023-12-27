# webpack配置app和pc多入口


> 需求：原先做的app端又增加了pc端，现在把他们放在同一个项目下，就需要我们配置多入口了。

<!-- more -->

* 原先页面为index.html,后面增加pcJs.html


## 更改路径
在paths.js里
```javascript
{
// 增加pc的html
  pcIndexJs: resolveModule(resolveApp, 'src/pc/index'),
}
```
## 增加entry
在webpack.config.js下
```javascript
entry: {
      index: [
        isEnvDevelopment &&
          require.resolve("react-dev-utils/webpackHotDevClient"),
        paths.appIndexJs
      ].filter(Boolean),
      pcJs: [
        isEnvDevelopment &&
          require.resolve("react-dev-utils/webpackHotDevClient"),
        paths.pcIndexJs
      ].filter(Boolean)
    },
```
## 动态生成文件名
在webpack.config.js下
```javascript
output: {
     filename: isEnvProduction
        ? "static/js/[name].[contenthash:8].js"
        : isEnvDevelopment && "static/js/[name].bundle.js",
      // TODO: remove this when upgrading to webpack 5
      futureEmitAssets: true,
      // There are also additional JS chunk files if you use code splitting.
      chunkFilename: isEnvProduction
        ? "english/static/js/[name].[contenthash:8].chunk.js"
        : isEnvDevelopment && "static/js/[name].chunk.js",
}
```
## 动态生成对应HTML
在webpack.config.js下
```javascript
plugins: [
    new HtmlWebpackPlugin(
        Object.assign(
          {},
          {
            inject: true,
            template: paths.appHtml,
            chunks: ["index"],
            inject: true,
            filename: "index.html"
          },
          isEnvProduction
            ? {
                minify: {
                  removeComments: true,
                  collapseWhitespace: true,
                  removeRedundantAttributes: true,
                  useShortDoctype: true,
                  removeEmptyAttributes: true,
                  removeStyleLinkTypeAttributes: true,
                  keepClosingSlash: true,
                  minifyJS: true,
                  minifyCSS: true,
                  minifyURLs: true
                }
              }
            : undefined
        )
      ),
    new HtmlWebpackPlugin(
            Object.assign(
            {},
            {
                inject: true,
                template: paths.appHtml,
                chunks: ["pcJs"],
                inject: true,
                filename: "pcJs.html"
            },
            isEnvProduction
                ? {
                    minify: {
                    removeComments: true,
                    collapseWhitespace: true,
                    removeRedundantAttributes: true,
                    useShortDoctype: true,
                    removeEmptyAttributes: true,
                    removeStyleLinkTypeAttributes: true,
                    keepClosingSlash: true,
                    minifyJS: true,
                    minifyCSS: true,
                    minifyURLs: true
                    }
                }
                : undefined
            )
        ),
]
```
## 路由重指向
在webpackDevServer.config.js下
```javascript
 historyApiFallback: {
      // Paths with dots should still use the history fallback.
      // See https://github.com/facebook/create-react-app/issues/387.
      disableDotRule: true,
      // pc开头全部指向pcJs.html
      rewrites: [{ from: /^\/pc/, to: "/pcJs.html" }]
    },
```
**重定向时to的后面，不需要加build!**

