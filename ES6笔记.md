
#### 零碎知识点

##### 字符串处理

* codePointAt方法测试一个字符由两个字节还是由四个字节组成.

```javascript
    function is32Bit(c) {
        return c.codePointAt(0) > 0xFFFF;
    }
    
    is32Bit("𠮷")23  // true
    is32Bit("a") // false
```

* padStart()，padEnd() 字符串头尾补全, 接受两个参数，第一个参数用来指定字符串的最小长度，第二个参数是用来补全的字符串。
* 模板字符串 可以调用函数, 嵌套。模板字符里面有变量，先处理成多个参数，再调用函数。
                             
```javascript
    $('#result').append(`
      There are <b>${basket.count}</b> items
       in your basket, <em>${basket.onSale}</em>
      are on sale!
    `);

    $('#list').html(`
    <ul>
      <li>first</li>
      <li>second</li>
    </ul>
    `.trim());  // 所有模板字符串的空格和换行，都是被保留的，比如<ul>标签前面会有一个换行, 可以使用trim方法消除它。
    
    alert`123`; // 等同于  alert(123)
    
    String.raw({ raw: 'test' }, 0, 1, 2);
    // 't0e1s2t'
    
    // 等同于
    String.raw({ raw: ['t','e','s','t'] }, 0, 1, 2);
    
```

##### 正则
* ES6 为正则表达式新增了flags属性，会返回正则表达式的修饰符。
* 后行断言

```javascript
    //s 修饰符：dotAll 模式
    /foo.bar/s.test('foo\nbar'); // true  引入/s修饰符，使得.可以匹配任意单个字符。
    
    //先行断言
    /\d+(?=%)/.exec('100% of US presidents have been male');  // ["100"]
    /\d+(?!%)/.exec('that’s all 44 of them');                // ["44"]
    
    //后行断言
    /(?<=\$)\d+/.exec('Benjamin Franklin is on the $100 bill');  // ["100"]
    /(?<!\$)\d+/.exec('it’s is worth about €90');                // ["90"]
    
    //后行断言结果不一样
    /(?<=(\d+)(\d+))$/.exec('1053'); // ["", "1", "053"]
    /^(\d+)(\d+)$/.exec('1053'); // ["1053", "105", "3"]
```

##### 数值
* Number方法转换十进制
* Math.trunc() 去除一个数的小数部分,返回整数部分.内部先使用Number方法转为数值, 对于空值和无法截取整数的值，返回NaN。
* Math.sign() 方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。无法转为数值的值，会返回NaN。
* Math.cbrt() 计算立方根。
* \*\* 指数运算符 `2 ** 3 // 8`。
* Integer 类型不能与 Number 类型进行混合运算。`1n + 1 // 报错`。
  
```javascript
    Number('0b111');  // 7
    Number('0o10');  // 8
    
    //Math.sign()
    Math.sign(-5) // -1
    Math.sign(5) // +1
    Math.sign(0) // +0
    Math.sign(-0) // -0
    Math.sign(NaN) // NaN
    
    //指数运算符
    let a = 1.5;
    a **= 2;
    // 等同于 a = a * a;
    
    let b = 4;
    b **= 3;
    // 等同于 b = b * b * b;
```

##### 函数
* 参数变量是默认声明的，所以不能用let或const再次声明。使用参数默认值时，函数不能有同名参数。 参数默认值是惰性求值.
* rest 用于获取函数的多余参数，不需要使用arguments对象, 参数之后不能再有其他参数, 函数的length属性，不包括 rest 参数。
* 箭头函数 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
    1. 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
    2. 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
    3. 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
    4. 不可以使用yield命令，因此箭头函数不能用作 Generator 函数。
    5. 箭头函数没有自己的this，所以当然也就不能用call()、apply()、bind()这些方法去改变this的指向。
* 双冒号运算符 双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象，作为上下文环境（即this对象），绑定到右边的函数上面。如果双冒号左边为空，右边是一个对象的方法，则等于将该方法绑定在该对象上面。
* ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。
* 箭头函数表达式的语法比函数表达式更短，并且没有自己的this，arguments，super或 new.target。这些函数表达式更适用于那些本来需要匿名函数的地方，并且它们不能用作构造函数。

```javascript

    // arguments量变的写法
    function sortNumbers() {
        return Array.prototype.slice.call(arguments).sort();    // Array.prototype.slice.call先将arguments转为数组。
    }
    
    // rest参数的写法
    const sortNumbers = (...numbers) => numbers.sort();
    
    // 箭头函数
    var sum = (num1, num2) => { return num1 + num2; }
    
    let getTempItem = id => ({ id: id, name: 'emp' });
    
    // 双冒号运算符
    foo::bar; // 等同于 bar.bind(foo);
    
    foo::bar(...arguments); // 等同于 bar.apply(foo, arguments)
    
    // 非递归的斐波那契数列
    function Fibonacci (n) {
      if ( n <= 1 ) {return 1};
    
      return Fibonacci(n - 1) + Fibonacci(n - 2);
    }
    
    Fibonacci(10) // 89
    Fibonacci(100) // 堆栈溢出
    Fibonacci(500) // 堆栈溢出
    
    // 尾递归斐波那契数列
    function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
      if( n <= 1 ) {return ac2};
    
      return Fibonacci2 (n - 1, ac2, ac1 + ac2);
    }
    
    Fibonacci2(100) // 573147844013817200000
    Fibonacci2(1000) // 7.0330367711422765e+208
    Fibonacci2(10000) // Infinity
    
    // 蹦床函数（trampoline）可以将递归执行转为循环执行。
    function trampoline(f) {    //
      while (f && f instanceof Function) {
        f = f();
      }
      return f;
    }
    
```

##### 数组的扩展
* 扩展运算符(...) 好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。替代apply方法。若将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。会将空位转为undefined
* Array.of方法用于将一组值，转换为数组。
* 数组实例的copyWithin方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。                                                                               

```javascript
    // 扩展运算符
    console.log(...[1,2,3])     // 1 2 3
    
    Math.max(...[14, 3, 77])    // 相当于 Math.max.apply(null, [14, 3, 77])
    
    // push方法
    
    // es5
    var arr1 = [0, 1, 2];
    var arr2 = [3, 4, 5];
    Array.prototype.push.apply(arr1, arr2);
    
    // es6
    let arr1 = [0, 1, 2];
    let arr2 = [3, 4, 5];
    arr1.push(...arr2);
    
    // 合并数组
    var arr1 = ['a', 'b'];
    var arr2 = ['c'];
    var arr3 = ['d', 'e'];
    
    // ES5的合并数组
    arr1.concat(arr2, arr3);
    // [ 'a', 'b', 'c', 'd', 'e' ]
    
    // ES6的合并数组
    [...arr1, ...arr2, ...arr3]
    // [ 'a', 'b', 'c', 'd', 'e' ]
    
    [1, 2, 3, 4, 5].copyWithin(0, 3)
    
    [1, 4, -5, 10].find((n) => n < 0)   // -5

```

##### 对象的扩展
* 属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串[object Object].
* Object.assign()用于对象的合并. 只有字符串的包装对象会产生可枚举属性. 浅拷贝.  
* 属性遍历的次序规则
    1. 首先遍历所有数值键，按照数值升序排列。
    2. 其次遍历所有字符串键，按照加入时间升序排列。
    3. 最后遍历所有 Symbol 键，按照加入时间升序排列。
* super，指向当前对象的原型对象。表示原型对象时, 只能用在对象的方法中.`super.foo`等同于`Object.getPrototypeOf(this).foo`(属性) 或 `Object.getPrototypeOf(this).foo.call(this)`(方法).
* Null 传导运算符 
    1. obj?.prop // 读取对象属性
    2. obj?.[expr] // 同上
    3. func?.(...args) // 函数或对象方法的调用
    4. new C?.(...args) // 构造函数的调用
    
```javascript
    
    // 克隆对象保持继承链
    function clone(origin) {
      let originProto = Object.getPrototypeOf(origin);
      return Object.assign(Object.create(originProto), origin);
    }
    
    // 混入模式
    let mix = (object) => ({
        with: (...minin) => Object.reduce(
            (c, mixin) => Object.create(
                c, Object.getOwnPropertyDescriptors(mixin)
            ), object)
    });

    // 传导运算符
    const firstName = message?.body?.user?.firstName || 'default';  // 只要其中一个返回null或undefined 返回undefined

```

##### Set 和 Map 数据结构
* Set 类似于数组，但是成员的值都是唯一的，没有重复的值。本身是一个构造函数，用来生成 Set 数据结构。在Set内部, 两个NaN是相等的, 两个对象总是不相等的. 四个方法 `add(value)  delete(value)  has(value)  clear()`
* WeakSet 结构与 Set 类似, 成员只能是对象. 不能遍历(undefined).
* Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。方法有 `size() set(key, value) get(key) has(key) delete(key) clear()`.
* WeakMap结构与Map结构类似，也是用于生成键值对的集合。
    1. 和Map区别: WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。WeakMap的键名所指向的对象，不计入垃圾回收机制。

```javascript
    const s = new Set();
    [2, 3, 4, 5, 3, 5].forEach(x => s.add(x));
    
    for (let i of s) {
        console.log(i);
    }
    // 2 3 4 5
    
    // 数组去重
    const set = new Set([1, 2, 3, 4, 4]);
    [...set]
    
    // Array.from方法可以将 Set 结构转为数组
    function dedupe(array) {
      return Array.from(new Set(array));
    }
    
    dedupe([1, 1, 2, 3]) // [1, 2, 3]    
    
    // Map  未使用Map方法
    const data = {};
    const element = document.getElementById('myDiv');
    
    data[element] = 'metadata';
    data['[object HTMLDivElement]'] // "metadata"
    
    // 使用Map方法
    const m = new Map();
    const o = {p: 'Hello World'};
    
    m.set(o, 'content');
    m.get(o);
    
    m.has(o) // true
    m.delete(o) // true
    m.has(o) // false
    
    // Map 转为数组最方便的方法，就是使用扩展运算符（...）
    const myMap = new Map()
        .set(true, 7)
        .set({foo: 3}, ['abc']);
    [...myMap];
    // [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
    
    // 数组转Map
    new Map([
        [true, 7],
        [{foo: 3}, ['abc']]
    ]);
    
    // Map转为对象
    function strMapToObj(strMap) {
      let obj = Object.create(null);
      for (let [k,v] of strMap) {
        obj[k] = v;
      }
      return obj;
    }
    
    const myMap = new Map()
      .set('yes', true)
      .set('no', false);
    strMapToObj(myMap);
    // { yes: true, no: false }
    
    // 对象转为 Map
    function objToStrMap(obj) {
      let strMap = new Map();
      for (let k of Object.keys(obj)) {
        strMap.set(k, obj[k]);
      }
      return strMap;
    }
    
    objToStrMap({yes: true, no: false});
    // Map {"yes" => true, "no" => false}
    
    // Map转JSON
    // 1. Map 的键名都是字符串，这时可以选择转为对象 JSON
    function strMapToJson(strMap) {
      return JSON.stringify(strMapToObj(strMap));       // 注意前面的strMapTOObj方法
    }
    
    let myMap = new Map().set('yes', true).set('no', false);
    strMapToJson(myMap);
    // '{"yes":true,"no":false}'
    
    // 2. Map 的键名有非字符串，这时可以选择转为数组 JSON
    function mapToArrayJson(map) {
      return JSON.stringify([...map]);
    }
    
    let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
    mapToArrayJson(myMap);
    // '[[true,7],[{"foo":3},["abc"]]]'
    
    // JSON 转为 Map 
    // 正常情况下，所有键名都是字符串
    function jsonToStrMap(jsonStr) {
      return objToStrMap(JSON.parse(jsonStr));      // // 注意前面的objToStrMap方法
    }
    
    jsonToStrMap('{"yes": true, "no": false}');
    // Map {'yes' => true, 'no' => false}
    
    // 特殊情况
    function jsonToMap(jsonStr) {
      return new Map(JSON.parse(jsonStr));
    }
    
    jsonToMap('[[true,7],[{"foo":3},["abc"]]]');
    // Map {true => 7, Object {foo: 3} => ['abc']}
    
```
