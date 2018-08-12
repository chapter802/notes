---
title: webpack文档摘要
tags: webpack
---
## 说明
### 字体颜色
* <b style="color: #ED7578">红色</b> 标识为 <b style="color: #ED7578">重点</b>
* <b style="color: #97CA97">绿色</b> 标识为 <b style="color: #97CA97">技巧</b> 
* <b style="color: #6190BF">蓝色</b> 标识为 <b style="color: #6190BF">提示</b> 
* <b style="color: #F79057">黄色</b> 标识为 <b style="color: #F79057">警告</b> 


## 指南部分
### webpack常用插件 
* html-webpack-plugin
* clean-webpack-plugin //文件管理插件

### source map查错 
* `devtool:‘inline-source-map’   //位置 webpack.config.js`

### 代码自动化编译 
1. 使用观察模式 
 * 方法: package.json 文件script部分添加`"watch": "webpack --watch"`
2. 使用 webpack-dev-server 
  * 方法: 修改webpack.config.js告诉开发服务器(dev server)，在哪里查找文件：` devServer:{contentBase: './dist'},` 在pakage.json文件script部分添加`"start": "webpack-dev-server --open"`,然后直接运行`npm start`
3. 使用 webpack-dev-middleware

### 模块热替换 只适合生产环境

### 生产环境构建
1. 自动方式
 * webpack-p相当于`webpack --optimize-minimize --define process.env.NODE_ENV="'production'"`
    * 用UglifyJsPlugin压缩js,
    * 运行[LoaderOptionsPlugin](https://doc.webpack-china.org/plugins/loader-options-plugin/),
    * 设置 NodeJS 环境变量，触发某些 package 包，以不同的方式进行编译。
     
    <!-- more -->
2. 手动方式
 * 定义两个完全独立的配置文件
 **webppack.dev.js**
 
     ```Javascript
    module.exports = {
      devtool: 'cheap-module-source-map',
    
      output: {
        path: path.join(__dirname, '/../dist/assets'),
        filename: '[name].bundle.js',
        publicPath: publicPath,
        sourceMapFilename: '[name].map'
      },
    
      devServer: {
        port: 7777,
        host: 'localhost',
        historyApiFallback: true,
        noInfo: false,
        stats: 'minimal',
        publicPath: publicPath
      }
    }
    ```
    **webpack.prod.js**
        
    ```Javascript
    module.exports = {
      output: {
        path: path.join(__dirname, '/../dist/assets'),
        filename: '[name].bundle.js',
        publicPath: publicPath,
        sourceMapFilename: '[name].map'
      },
    
      plugins: [
        new webpack.LoaderOptionsPlugin({
          minimize: true,
          debug: false
        }),
        new webpack.optimize.UglifyJsPlugin({
          beautify: false,
          mangle: {
            screw_ie8: true,
            keep_fnames: true
          },
          compress: {
            screw_ie8: true
          },
          comments: false
        })
      ]
    }
    ```

     **pakage.json** 
        
    ```Javascript
    "scripts": {
    ...
    "build:dev": "webpack --env=dev --progress --profile --colors",
    "build:dist": "webpack --env=prod --progress --profile --colors"
    }
    ```
    **webpack.config.js**
    
    ```Javascript
    module.exports = function(env) {
  return require(`./webpack.${env}.js`)
}

    ```
    了解 [env](https://doc.webpack-china.org/api/cli/#common-options)
    
    * 高级途径 [详见>>](https://doc.webpack-china.org/guides/production/)   
     一个基本配置文件，包含两个环境通用的配置，然后将其与特定于环境的配置用 [webpack-merge](https://github.com/survivejs/webpack-merge) 进行合并。这将为每个环境产生完整配置，并防止重复公共部分代码。

### Tree Shaking
* 从maths.js 引入cube方法 ` import {cube} from './maths.js';`

### 代码分离
1. 入口起点：使用 [entry](https://doc.webpack-china.org/configuration/entry-context) 选项手动分离代码。  
    缺点: 会引起代码重复, 动态分离代码缺乏弹性.
2. 防止重复：使用 [CommonsChunkPlugin](https://doc.webpack-china.org/plugins/commons-chunk-plugin) 去重和分离 chunk。
 * [ExtractTextPlugin](https://doc.webpack-china.org/plugins/extract-text-webpack-plugin) : 分离CSS
 * [bundle-loader](https://doc.webpack-china.org/loaders/bundle-loader) : 分离代码,懒加载结果包
 * [promise-loader](https://github.com/gaearon/promise-loader) : 类似bundle-loader,用promises方法
3. 动态导入：通过模块的内联函数调用来分离代码。
 * AsyncFunciton `new AsyncFunction([arg1[, arg2[, ...argN]],] functionBody)`

    ```Javascript
function resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}
var AsyncFunction = Object.getPrototypeOf(async function(){}).constructor;
var a = new AsyncFunction('a', 'b', 'return await resolveAfter2Seconds(a) + await resolveAfter2Seconds(b);');
a(10, 20).then(v => {
  console.log(v); // prints 30 after 4 seconds
});
    ```
4. Bundle Analysis  
   [webpack-chart](https://github.com/alexkuz/webpack-chart) ,[webpack-visualizer](https://chrisbateman.github.io/webpack-visualizer/) .

### 缓存
确保编译好的文件能被客户端缓存, 文件内容变化后能够请求到新文件.
* 输出文件名 //<b style="color: #6190BF">通过 output.filename 使用 [chunkhash] 替换 filename: '[name].[chunkhash].js'. </b> 将第三方库(library)（例如 lodash 或 react）提取到单独的 vendor chunk 文件中，是比较推荐的做法.
* 提取模板 // [CommonsChunkPlugin](https://doc.webpack-china.org/plugins/commons-chunk-plugin)
* 模块标识符 // [HashedModuleIdsPlugin](https://doc.webpack-china.org/plugins/hashed-module-ids-plugin) 用于生产环境

### 未完待续...
---

## 文档部分
### 配置 (Configuration)
安装配置Babel和JSX  
` npm install --save-dev babel-register jsxobj babel-preset-es2015 `  

.babelrc

` "presets": [ "es2015" ] `
以React为例

```Javascript
import jsxobj from 'jsxobj';

// example of an imported plugin
const CustomPlugin = config => ({
  ...config,
  name: 'custom-plugin'
});

export default (
  <webpack target="web" watch>
    <entry path="src/index.js" />
    <resolve>
      <alias {...{
        react: 'preact-compat',
        'react-dom': 'preact-compat'
      }} />
    </resolve>
    <plugins>
      <uglify-js opts={{
        compression: true,
        mangle: false
      }} />
      <CustomPlugin foo="bar" />
    </plugins>
  </webpack>
);
```
### 入口和上下文 (Entry and Context)
* <b style="color: #ED7578">context</b> 基础目录, **绝对路径**, 用于从配置中解析入口起点(entry point)和 loader  
  `context: path.resolve(__dirname, "app")`   
* <b style="color: #ED7578">entry</b> 每个 HTML 页面都有一个入口起点。单页应用(SPA)：一个入口起点，多页应用(MPA)：多个入口起点。
* <b style="color: #ED7578">命名</b> 如果传入一个字符串或字符串数组，chunk 会被命名为 main。如果传入一个对象，则每个键(key)会是 chunk 的名称，该值描述了 chunk 的入口起点。
* <b style="color: #ED7578">动态入口</b>  
    `entry: () => './demo'`  或者
    ` entry: () => new Promise((resolve) => resolve(['./demo', './demo2']))`
    
### 输出 (Output)
* output.auxiliaryComment 和 output.library 和 output.libraryTarget 一起使用向导出容器插入注释.

  ```Javascript
      auxiliaryComment: {
      root: "Root Comment",
      commonjs: "CommonJS Comment",
      commonjs2: "CommonJS2 Comment",
      amd: "AMD Comment"
    }
   ```
   
* <b style="color: #ED7578">output.chunkFilename / output.filename</b>
  1. 单个 `filename: "bundle.js"`
  2. 多个 `filename: "[name].bundle.js"`
  3. 使用内部chunk id `filename: "[id].bundle.js"`
  4. 每次构建生成唯一hash `filename: "[name].[hash].bundle.js"`
  5. 基于每个 chunk 内容的 hash `filename: "[chunkhash].bundle.js"`
* output.crossOriginLoading 跨域加载  
  1. false - 禁用跨域加载（默认）
  2. "anonymous" - 不带凭据(credential)启用跨域加载
  3. "use-credentials" - 带凭据(credential)启用跨域加载 with credentials
* output.hashDigestLength //散列摘要的前缀长度，默认为 20
* output.hashFunction //散列算法，默认为 'md5'
* <b style="color: #ED7578">output.library / output.libraryTarget</b>
 
    1.`libraryTarget: "var"`- （默认值）当 library 加载完成，入口起点的返回值将分配给一个变量： 
    
    ```Javascript
      var MyLibrary = _entry_return_;
    
       // 使用者将会这样调用你的 library
       MyLibrary.doSomething();
    ```

    2.`libraryTarget: "this"` - 当 library 加载完成，入口起点的返回值将分配给 this，this 的含义取决于你：

    <pre>
    this["MyLibrary"] = _entry_return_;
       // 使用者将会这样调用你的library
       this.MyLibrary.doSomething();
       MyLibrary.doSomething(); //如果 this 是window
    </pre>
      
    3.`libraryTarget: "window"` - 当 library 加载完成，入口起点的返回值将分配给 window 对象。
    4.`libraryTarget: "global"` - 当 library 加载完成，入口起点的返回值将分配给 global 对象。
    5.`libraryTarget: "commonjs" / "commonjs2"` - 当 library 加载完成，入口起点的返回值将分配给 exports 对象。这个名称也意味着模块用于 CommonJS 环境。
    6.`libraryTarget: "amd"` - webpack 将你的 library 转为 AMD 模块。入口 chunk 必须使用 define 属性定义，如果不是，webpack 将创建无依赖的 AMD 模块。使用如下配置：
    `output: {library: "MyLibrary",libraryTarget: "amd"`
    7.`libraryTarget: "umd"`- 可以将你的 library 能够在所有的模块定义下都可运行。  ibrary 属性来命名你的模块：
    
    ```Javascript
    output: {
        library: "MyLibrary",
        libraryTarget: "umd"
    }
           
    输出：
           
    (function webpackUniversalModuleDefinition(root,    factory) {
      if(typeof exports === 'object' && typeof module === 'object')
      module.exports = factory();
      else if(typeof define === 'function' && define.amd)
      define([], factory);
      else if(typeof exports === 'object')
          exports["MyLibrary"] = factory();
      else
          root["MyLibrary"] = factory();
      })(this, function() {
      //这个模块会返回你的入口 chunk 所返回的
      });
    ```
  webpack 3.1.0 开始，可以将 library 指定为一个对象，用于对每个 target 起不同的名称：
  
    ```Javascript
         output: {
             library: {
               root: "MyLibrary",
               amd: "my-library",
               commonjs: "my-common-library"
             },
             libraryTarget: "umd"
           }
    ```
     8.`libraryTarget: "assign"` - 这里 webpack 会轻率地产生隐含的全局变量。 如果前面没有定义 MyLibrary，则 library 将被设置在全局范围内。`MyLibrary = _entry_return_;`     
     9.`libraryTarget: "jsonp"`把入口起点的返回值，包裹到一个 jsonp 包装容器中，library 的依赖将由 [externals](https://doc.webpack-china.org/configuration/externals/) 配置定义。`MyLibrary(_entry_return_);` 
* output.path //output 目录对应一个绝对路径。
* <b style="color: #ED7578">output.publicPath</b>
  对于按需加载(on-demand-load)或加载外部资源(external resources)（如图片、文件等）来说，output.publicPath 是很重要的选项。如果指定了一个错误的值，则在加载这些资源时会收到 404 错误。
  该选项的值是以 runtime(运行时) 或 loader(载入时) 所创建的每个 URL 为前缀。因此，在多数情况下，此选项的值都会以/结束。
  <b style="color: #6190BF">默认值是一个空字符串 ""</b>
  简单规则如下： `output.path` 中的 URL 以 HTML 页面为基准。
  
     ```Javascript
       path: path.resolve(__dirname, "public/assets"),
       publicPath: "https://cdn.example.com/assets/"
     ```
     ```Javascript
     配置示例：
        publicPath: "https://cdn.example.com/assets/", // CDN（总是 HTTPS 协议）
        publicPath: "//cdn.example.com/assets/", // CDN (协议相同)
        publicPath: "/assets/", // 相对于服务(server-relative)
        publicPath: "assets/", // 相对于 HTML 页面
        publicPath: "../assets/", // 相对于 HTML 页面
        publicPath: "", // 相对于 HTML 页面（目录相同）
      ```

* output.umdNamedDefine
  当使用了 libraryTarget: "umd"，设置：`umdNamedDefine: true` 会对 UMD 的构建过程中的 AMD 模块进行命名。否则就使用匿名的 define。
  
### 模块(Module)
  * module.noParse  
    防止 webpack 解析那些任何与给定正则表达式相匹配的文件。忽略的文件中不应该含有 import, require, define 的调用，或任何其他导入机制。忽略大型的 library 可以提高构建性能。
    
    ```Javascript
        noParse: /jquery|lodash/

        // 从 webpack 3.0.0 开始
        noParse: function(content) {
          return /jquery|lodash/.test(content);
        }
    ```
#### Rule (condition / result / nested rule)

1. 条件(condition)
   两种输入值。在规则中，属性 test, include, exclude 和 resource 对 resource 匹配，并且属性 issuer 对 issuer 匹配
 * resource：请求文件的绝对路径。
 * issuer: 被请求资源(requested the resource)的模块文件的绝对路径。是导入时的位置。  
2. 结果(result)  
   规则结果只在规则条件匹配时使用。规则有两种输入值：
   * 应用的 loader：应用在 resource 上的 loader 数组。
   * Parser 选项：用于为模块创建解析器的选项对象。  
<b style="color: #F79057">注: </b>  这些属性会影响 loader：loader, options, use。也兼容这些属性：query, loaders。
  enforce 属性会影响 loader 种类。不论是普通的，前置的，后置的 loader。  parser 属性会影响 parser 选项。
3. 嵌套的 Rule(nested rule)
   * Rule.enforce //可能的值有："pre" | "post"
      - 指定 loader 种类。没有值表示是普通 loader。
      - 还有一个额外的种类"行内 loader"，loader 被应用在 import/require 行内。
      - 所有 loader 通过 后置, 行内, 普通, 前置 排序，并按此顺序使用。所有普通 loader 可以通过在请求中加上 ! 前缀来忽略（覆盖）。
      - 所有普通和前置 loader 可以通过在请求中加上 -! 前缀来忽略（覆盖）。
      - 所有普通，后置和前置 loader 可以通过在请求中加上 !! 前缀来忽略（覆盖）。
      - 不应该使用行内 loader 和 ! 前缀，因为它们是非标准的。它们可在由 loader 生成的代码中使用。
   * Rule.oneOf //规则数组，当规则匹配时，只使用第一个匹配规则。
   * Rule.parser //解析选项对象。所有应用的解析选项都将合并。
      示例（默认的插件解析器选项）：
    
      ```Javascript
        parser: {
          amd: false, // 禁用 AMD
          commonjs: false, // 禁用 CommonJS
          system: false, // 禁用 SystemJS
          harmony: false, // 禁用 ES2015 Harmony import/export
          requireInclude: false, // 禁用 require.include
          requireEnsure: false, // 禁用 require.ensure
          requireContext: false, // 禁用 require.context
          browserify: false, // 禁用特殊处理的 browserify bundle
          requireJs: false, // 禁用 requirejs.*
          node: false, // 禁用 __dirname, __filename, module, require.extensions, require.main 等。
          node: {...} // 在模块级别(module level)上重新配置 node 层(layer)
        }
      ```
      
   * Rule.use //应用于模块的 UseEntries 列表。每个入口(entry)指定使用一个 loader。
      示例: 
      
      ```Javascript
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              importLoaders: 1
            }
          },
          {
            loader: 'less-loader',
            options: {
              noIeCompat: true
            }
          }
        ]
      ```
      
#### 条件 

* 字符串：匹配输入必须以提供的字符串开始。是的。目录绝对路径或文件绝对路径。
* 正则表达式：test 输入值。
* 函数：调用输入的函数，必须返回一个真值(truthy value)以匹配。
* 条件数组：至少一个匹配条件。
* 对象：匹配所有属性。每个属性都有一个定义行为。
   { test: Condition }：匹配特定条件。一般是提供一个正则表达式或正则表达式的数组，但这不是强制的。
   
   { include: Condition }：匹配特定条件。一般是提供一个字符串或者字符串数组，但这不是强制的。
   
   { exclude: Condition }：排除特定条件。一般是提供一个字符串或字符串数组，但这不是强制的。
   
   { and: [Condition] }：必须匹配数组中的所有条件
   
   { or: [Condition] }：匹配数组中任何一个条件
   
   { not: [Condition] }：必须排除这个条件

#### UseEntry

必须有一个 loader 属性是字符串。它使用 loader 解析选项（resolveLoader），相对于配置中的 context 来解析。
示例: 

```Javascript
   {
     loader: "css-loader",
     options: {
       modules: true
     }
   }
```

<b style="color: #F79057">注: </b> webpack 需要生成资源和所有 loader 的独立模块标识，包括选项。它尝试对选项对象使用 JSON.stringify。

### 解析(Resolve)

设置模块如何被解析。

* <b style="color: #ED7578">resolve.alias</b> 
  创建 import 或 require 的别名，来确保模块引入变得更简单。
  例如，一些位于 src/ 文件夹下的常用模块：
  
    ```Javascript
    alias: {
     Utilities: path.resolve(__dirname, 'src/utilities/'),
     Templates: path.resolve(__dirname, 'src/templates/')
    }
    ```
    现在，替换「在导入时使用相对路径」这种方式，就像这样：
    
    ```Javascript
        import Utility from '../../utilities/utility';
    ```
    使用别名：
    
    ```Javascript
        import Utility from 'Utilities/utility';
    ```
    也可以在给定对象的键后的末尾添加 $，以表示精准匹配：
    
    ```Javascript
        alias: {
          xyz$: path.resolve(__dirname, 'path/to/file.js')
        }
    ```
    产生以下结果：
    
    ```Javascript
        import Test1 from 'xyz'; // 精确匹配，所以 path/to/file.js 被解析和导入
        import Test2 from 'xyz/file.js'; // 精确匹配，触发普通解析
    ```
 
 * resolve.enforceExtension   默认值为false  
   如果是 true，将不允许无扩展名(extension-less)文件。默认如果 ./foo 有 .js 扩展，require('./foo') 可以正常运行。但如果启用此选项，只有 require('./foo.js') 能够正常工作。
 * resolve.enforceModuleExtension  默认值为false  
   对模块是否需要使用的扩展（例如 loader）。
 * resolve.extensions  默认值为：
 
    ```Javascript
       extensions: [".js", ".json"]
    ```
    自动解析确定的扩展。能够使用户在引入模块时不带扩展:
   
    ```Javascript
       import File from '../path/to/file'
    ```
    <b style="color: #F79057">注: </b> 使用此选项，会覆盖默认数组 webpack 将不再尝试使用默认扩展来解析模块。对于使用其扩展导入的模块，例如，import SomeFile from "./somefile.ext"，要想正确的解析，一个包含“*”的字符串必须包含在数组中。  
 * resolve.mainFields   
    当从 npm 包中导入模块时，此选项将决定在 package.json 中使用哪个字段导入模块。根据 webpack 配置中指定的 target 不同，默认值也会有所不同。
    
    ```Javascript
        mainFields: ["browser", "module", "main"]
    ```
    对于其他任意的 target（包括 node），默认值为：
    
    ```Javascript
        mainFields: ["module", "main"]
    ```
    
   * resolve.mainFiles  
     解析目录时要使用的文件名。默认：
     
     ```Javascript
         mainFiles: ["index"]
     ```
     
   * resolve.modules  
     webpack 解析模块时搜索的目录。默认：
     
     ```Javascript
        modules: ["node_modules"]
     ```
     添加一个目录到模块搜索目录，此目录优先于 node_modules/ 搜索：
     
     ```Javascript
         modules: [path.resolve(__dirname, "src"), "node_modules"]
     ```

* resolve.plugins  
  使用的额外的解析插件列表。它允许插件，如 [DirectoryNamedWebpackPlugin](https://www.npmjs.com/package/directory-named-webpack-plugin)。
      
  ```Javascript
     plugins: [new DirectoryNamedWebpackPlugin()]
  ```
* resolveLoader  
  仅用于解析 webpack 的 loader 包。默认：
    ```Javascript
          {
              modules: ["node_modules"],
              extensions: [".js", ".json"],
              mainFields: ["loader", "main"]
           }
   ```
      
### 插件

webpack常用插件

### 开发中 Server(DevServer)

* <b style="color: #ED7578">devServer</b>
  例如对 `dist/`目录的文件都做 gzip 压缩和提供为服务：
  
    ```Javascript
      devServer: {
          contentBase: path.join(__dirname, "dist"),
          compress: true,
          port: 9000
        }
    ```
    
  * devServer.allowedHosts //定义一个hosts白名单来允许访问dev server。
  * devServer.compress  
    一切服务都启用gzip 压缩：`compress: true`  
    `webpack-dev-server --compress`
  * devServer.contentBase 
    告诉服务器从哪里提供内容。默认情况下，将使用当前工作目录作为提供内容的目录，可以修改为其他目录：
    
    ```Javascript
        contentBase: path.join(__dirname, "public")
    ```
    <b style="color: #F79057">注：</b> 推荐使用绝对路径。  
    也可以从多个目录提供内容：
    
     ```Javascript
         contentBase: [path.join(__dirname, "public"), path.join(__dirname, "assets")]
     ```
    禁用 contentBase：`contentBase: false`
    `webpack-dev-server --content-base /path/to/content/dir`
  * devServer.filename
    在惰性模式中，此选项可减少编译。 默认在惰性模式，每个请求结果都会产生全新的编译。使用 filename，可以只在某个文件被请求时编译。filename 使用如下：
    
    ```Javascript
        lazy: true,
        filename: "bundle.js"
    ```
     filename 在不使用惰性加载时没有效果。
  * devServer.headers //在所有响应中添加首部内容。
  * devServer.https 
     默认情况下，dev-server 通过 HTTP 提供服务。也可以选择带有 HTTPS 的 HTTP/2 提供服务：`https: true`
     使用以下设置自签名证书，但是你可以提供自己的：
     
     ```Javascrpt
         https: {
              key: fs.readFileSync("/path/to/server.key"),
              cert: fs.readFileSync("/path/to/server.crt"),
              ca: fs.readFileSync("/path/to/ca.pem"),
            }
     ```
     `webpack-dev-server --https`
     `webpack-dev-server --https --key /path/to/server.key --cert /path/to/server.crt --cacert /path/to/ca.pem`
  * <b style="color: #ED7578">devServer.inline</b> 
     在 dev-server 的两种不同模式之间切换。默认情况下，应用程序启用内联模式(inline mode)。这意味着一段处理实时重载的脚本被插入到你的包(bundle)中，并且构建消息将会出现在浏览器控制台。
     也可以使用 iframe 模式，它在通知栏下面使用 &lt;iframe&gt; 标签，包含了关于构建的消息。切换到 iframe 模式：`inline: false`
     `webpack-dev-server --inline=false`
<b style="color: #F79057">注：</b> 当使用模块热替换时，建议使用内联模式(inline mode)。
  * devServer.lazy //“惰性模式” 
      `webpack-dev-server --lazy` watchOptions 在使用惰性模式时无效。使用CLI，保内联模式(inline mode)被禁用。
  * devServer.open //开启时，dev server将打开浏览器。
  * devServer.overlay //浏览器显示警告和错误 
      `overlay: true` 
      
       ```Javascript
           overlay: {
              warnings: true,
              errors: true
            }
       ```
  * <b style="color: #ED7578">devServer.port
</b> //指定要监听请求的端口号。  
    `port: 8080`  / `webpack-dev-server --port 8080`
  * devServer.proxy
      在 localhost:3000 上有后端服务的话，你可以这样启用代理：
      
    ```Javascript
        proxy: {
            "/api": "http://localhost:3000"
            }
    ```
    如果你不想始终传递 /api ，则需要重写路径：
    
    ```Javascript
        proxy: {
          "/api": {
            target: "http://localhost:3000",
            pathRewrite: {"^/api" : ""}
          }
        }
    ```
    默认情况下，不接受运行在 HTTPS 上，且使用了无效证书的后端服务器。若使用，修改配置如下：
    
    ```Javascript
        proxy: {
          "/api": {
            target: "https://other-server.example.com",
            secure: false
          }
        }
    ```
    了解<b> [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware)</b>
    
* devServer.publicPath //此路径下的打包文件可在浏览器中访问。
* devServer.quiet //启用 quiet 后，除了初始启动信息之外的任何内容都不会被打印到控制台。
* devServer.watchContentBase // 开启后，文件改变页面将重载。`watchContentBase: true`

### 开发辅助调试工具(Devtool)

此选项控制是否生成，以及如何生成 source map。

#### 开发环境

* `eval` - 每个模块都使用 eval() 执行，并且都有 //@ sourceURL。此选项会相当快地构建。主要缺点是，由于会映射到转换后的代码，而不是映射到原始代码，所以不能正确的显示显示行数。
* `inline-source-map` - SourceMap 转换为 DataUrl 后添加到 bundle 中。
* `eval-source-map` - 每个模块使用 eval() 执行，并且 SourceMap 转换为 DataUrl 后添加到 eval() 中。初始化 SourceMap 时比较慢，但是会在重构建时提供很快的速度，并且生成实际的文件。行数能够正确映射，因为会映射到原始代码中。  
    和 eval-source-map 类似，每个模块都使用 eval() 执行。然而，使用此选项，Source Map 将传递给 eval() 作为 Data URL 调用。它是“低性能开销”的，因为它没有映射到列，只映射到行数。
* `cheap-module-eval-source-map` - 和 `cheap-eval-source-map `类似，然而，在这种情况下，loader 能够处理映射以获得更好的结果。

#### 生产环境

* `source-map` - 生成完整的 SourceMap，输出为独立文件。由于在 bundle 中添加了引用注释，所以开发工具知道在哪里去找到 SourceMap。
* `hidden-source-map`- 和 `source-map` 相同，但是没有在 bundle 中添加引用注释。如果你只想要 SourceMap 映射错误报告中的错误堆栈跟踪信息，但不希望将 SourceMap 暴露给浏览器开发工具。
* `cheap-source-map `- 不带列映射(column-map)的 SourceMap，忽略加载的 Source Map。
* `nosources-source-map` - 创建一个没有 sourcesContent 的 SourceMap。它可以用来映射客户端（译者注：指浏览器）上的堆栈跟踪，而不会暴露所有的源码。
    
### 构建目标(Targets)

告知 webpack 为目标(target)指定一个环境。

* 通过 WebpackOptionsApply ，可以支持以下字符串值： 
![WebpackOptionsApply](http://ov633cdh0.bkt.clouddn.com/option_apply.png) 

* function 
  如果传入一个函数，此函数调用时会传入一个 compiler 作为参数。如果以上列表中没有一个预定义的目标(target)符合你的要求，请将其设置为一个函数。
  不需要插件  
  
  ```Javascript
     const options = {
        target: () => undefined
    };
  ```
 用指定的插件

  ```Javascript
    const webpack = require("webpack");

    const options = {
      target: (compiler) => {
        compiler.apply(
          new webpack.JsonpTemplatePlugin(options.output),
          new webpack.LoaderTargetPlugin("web")
        );
      }
    };
  ```

### Watch 和 WatchOptions

watch模式默认关闭

* watchOptions.aggregateTimeout  
  当第一个文件更改，会在重新构建前增加延迟。这个选项允许 webpack 将这段时间内进行的任何其他更改都聚合到一次重新构建里。以毫秒为单位。
* watchOptions.ignored 
  忽略文件或者文件夹。`ignored: /node_modules/`   
  [anymatch](https://github.com/es128/anymatch) 模式 `ignored: "files/**/*.js"`
* watchOptions.poll   
  通过传递 true 开启 polling，或者指定毫秒为单位进行轮询。` poll: 1000 // 每秒检查一次变动`  

<b style="color: #F79057">注：</b> Watch 在 NFS 和 VirtualBox 机器上不适用。

---
## webpack相关常用插件和工具
1. [rscss](https://github.com/rstacruz/rscss) // 组件化开发可维护的CSS。
2. [css-modules](https://github.com/css-modules/css-modules) // 对CSS加入了局部作用域和模块依赖。  
  详见：阮一峰老师的 [CSS Modules 用法教程](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)
3. [postcss](https://github.com/postcss/postcss) // 通过自定义插件和工具这样的生态系统来改造CSS。
4. [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) // 自动生成一个 html 文件，并且引用相关的 assets 文件。
5. [UglifyJsPlugin]() // 压缩js文件。
6. [optimize-css-assets-webpack-plugin]() // 优化、压缩css；注：该插件是在ExtractTextPlugin输出css文件后，对文件进行再操作。
7. [webpack-hot-middleware]() // 热加载。
8. [CommonsChunkPlugin]() // 提取第三方代码，生成公共的js文件。
9. [DefinePlugin]() // 定义全局变量，供编译时使用
10. [extract-text-webpack-plugin]() // extract-text-webpack-plugin。
11. [BannerPlugin]() // 向chunk 文件头部添加注释。  
<b style="color: #F79057">注：</b> <b style="color: #F79057">5-11</b> 参考：简书 [webpack常用插件使用说明](http://www.jianshu.com/p/43661bc2adbc)

