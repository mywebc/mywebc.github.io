# 使用Rollup打包Vue或React组件库并发布到npm


> 最近在写一些Vue和React相关的组件库，涉及到打包文件发布npm这一块，在对比了**webpack**和**rollup**两种打包工具之后，决定使用rollup来打包，在此总结一下，希望可以给其他人参考。如果嫌我啰嗦，可以直接看代码，😊。


## Rollup
Rollup 是一个 JavaScript 模块打包器，可以将小块代码编译成大块复杂的代码，常用于打包类库，其配置项也非常的简洁,跟webpack其实差不多，一个最基本的配置格式大体如下：
```js
export default {
  // 入口
  input: "",
  // 输出
  output: [],
  // 排除项
  external: [],
  // 全局模块
  globals: {},
  // 插件
  plugins: [],
}
```

## 与webpack区别
### Rollup
* 简洁的API，易上手;
* 天生的Tree-shaking，自动删除冗余代码;
* 支持多模块导出;
* 能快速打出体积更小的bundle;
* 适合打包类、库；
### webpack
* 丰富的插件系统;
* 代码分割和静态资源导入;
* 热模块更新;
* 适合项目级应用;

## React
react 打包配置如下，大家可以根据自己的项目稍微修改下即可。
```js
import resolve from 'rollup-plugin-node-resolve';
import postcss from 'rollup-plugin-postcss';
import commonjs from 'rollup-plugin-commonjs';
import babel from 'rollup-plugin-babel';
import typescript from '@rollup/plugin-typescript';

const extensions = ['.js', '.jsx', '.ts', '.tsx'];

export default {
  input: 'src/components/index.tsx',
  output: [
    {
      name: 'index',
      format: 'es',
      file: 'dist/lib/react.esm.js'
    }, {
      name: 'index',
      format: 'umd',
      file: 'dist/lib/react.js'
    }
  ],
  external: ['react', 'react-dom'],
  globals: {
    react: 'React',
    "react-dom": "ReactDOM",
  },
  plugins: [
    resolve({
      mainFields: ['module', 'main', 'jsnext:main', 'browser'],
      extensions
    }),
    babel({
      exclude: '**/node_modules/**',
      runtimeHelpers: true
    }),
    commonjs({
      include: "node_modules/**"
    }),
    postcss({
      extract: true,
      extensions: ['.scss']
    }),
    typescript()
  ],
};
```

## Vue
vue 打包如下。
```js
import esbuild from 'rollup-plugin-esbuild'
import vue from 'rollup-plugin-vue'
import scss from 'rollup-plugin-scss'
import dartSass from 'sass';
import { terser } from "rollup-plugin-terser"

export default {
  input: 'src/lib/index.ts',
  output: [{
    globals: {
      vue: 'Vue'
    },
    name: 'vue',
    file: 'dist/lib/vue.js',
    format: 'umd',
    plugins: [terser()]
  }, {
    name: 'vue',
    file: 'dist/lib/vue.esm.js',
    format: 'es',
    plugins: [terser()]
  }],
  plugins: [
    scss({ include: /\.scss$/, sass: dartSass }),
    esbuild({
      include: /\.[jt]s$/,
      minify: process.env.NODE_ENV === 'production',
      target: 'es2015' 
    }),
    vue({
      include: /\.vue$/,
    })
  ],
}
```

##  发布npm
### 修改或添加package.json一些选项
```json
{
  "name": "你的组件库名字",
  "version": "组件库版本",
  "files": [
    "dist/lib/*"
  ],
  "main": "dist/lib/bundle.js",
  "module": "dist/lib/bundle.esm.js"
}
```

### 切换npm官方源
```js
// 查看
npm config get registry
// 设置
npm config set registry https://registry.npm.taobao.org
```

### 发布
```js
// 必须先登录
npm login
npm publish
```

### 注意
**保证库的名字唯一**

**保证每次发布的版本号不一样**


