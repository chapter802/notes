### 零碎知识点

#### 值
* 1/-0 -infinity
* 简单值（即标量基本类型值， scalar primitive） 总是 通过值复制的方式来赋值 / 传递，包括 null 、 undefined 、字符串、数字、布尔和 ES6 中的 symbol 。
* 复合值（compound value）——对象（包括数组和封装对象， 参见第 3 章）和函数，则 总 是 通过引用复制的方式来赋值 / 传递。
* void 运算符返回undefined
 
 ```javascript
    function foo(x) {
        x.push( 4 ); 
        x; // [1,2,3,4]
        // 然后 
        x = [4,5,6]; 
        x.push( 7 ); x; // [4,5,6,7]
    }
    
    var a = [1,2,3];
    foo( a );
    a; // 是[1,2,3,4]，不是[4,5,6,7]
```

* 异步js加载步骤 下载js组件模块 初始化请求参数 请求组件数据 拼装组织结构 绑定事件监听
* 深复制, JSON对象安全 可使用 `var newObj = JSON.parse(JSON.stringify(someObj))`
