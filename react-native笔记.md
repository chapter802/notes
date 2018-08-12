##### 零碎知识点

* AppRegistry
* React Native还支持WebSocket，这种协议可以在单个TCP连接上提供全双工的通信信道。
* NavigatorIOS
* 网络图片需要手动指定图片的尺寸
* <Imagebackground></Imagebackground> 类似背景图 background-image
* 动画值在不同的驱动方式之间是不能兼容的。因此如果你在某个动画中启用了原生驱动，那么所有和此动画依赖相同动画值的其他动画也必须启用原生驱动。
* setNativeProps就是等价于直接操作DOM节点的方法。在使用这个方法之前，请尽量先尝试用setState和shouldComponentUpdate方法来解决问题。
* 在Android上使用Stetho来调试
* 使用动画改变图片的尺寸时, 考虑到性能使用`transform: [{scale}]`的样式属性来改变尺寸。
* ScrollView必须有一个确定的高度才能正产工作, 不建议直接给ScrollView进行设置高度, ScrollView中不要加`{flex:1}`.
* ScrollView 内部的其他响应者尚无法阻止ScrollView本身成为响应者.

* `YellowBox.ignoreWarnings(['warning: '])`数组中的字符串就是要屏蔽的警告的开头的内容。会屏蔽掉所有以Warning开头的警告内容
* 发布应用时去除警告，报错

```JavaScript
    if(!__DEV__) {
      global.console = {
        info: () => {},
        log: () => {},
        warn: () => {},
        debug: () => {},
        error: () => {},
      }
    }
```

* 图片加载三种方式
    
    ```javascript
    // 1. 从当前项目中加载图片
      <Image source={require('./img/2.png')} style={{width:120, height:120}} />
      
      //2. 加载使用APP中的图片
      <Image source={require(imageicon_homepage_map)} />
      
      // 3. 加载来自网络的图片
      // resizeMode:Image.resizeMode.cover || Image.resizeMode.contain || Image.resizeMode.stretch 
      <Image source={{uri:'https://www.baidu.com/img.png'}} style={{flex:1; width:120,height:120, resizeMode:Image.resizeMode.cover}} />
    ```

* react native 




#### CSS流布局属性 

* 6个属性设置在容器上(flex-direction  flex-wrap  flex-flow  justify-content  align-items  align-content)
    1. flex-direction: row | row-reverse | column | column-reverse;
    2. flex-wrap: nowrap | wrap | wrap-reverse;
    3. flex-flow: <flex-direction> || <flex-wrap>;
    4. justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
    5. align-items: flex-start | flex-end | center | baseline | stretch;
    6. align-content: flex-start | flex-end | center | space-between | space-around | stretch;

* 6个属性设置在项目上 (order flex-grow flex-shrink flex-basis flex align-self)
    1. order: <integer>;
    2. flex-grow: <number>; /* default 0 */
    3. flex-shrink: <number>; /* default 1 */
    4. flex-basis: <length> | auto; /* default auto */
    5. flex: none | \[ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> \]
    6. align-self: auto | flex-start | flex-end | center | baseline | stretch;

##### 流布局知识点

* 设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。
* flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
* flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
* align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

* order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。
* align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。



 

#### 工具

* [Ignite CLI](https://github.com/infinitered/ignite)
* [React-Navigation](https://reactnavigation.org/)
* [NativeBase](https://github.com/GeekyAnts/NativeBase)
* [react-native-animatable](https://github.com/oblador/react-native-animatable)
* [react-native-image-header-scroll-view](https://github.com/bamlab/react-native-image-header-scroll-view)
* [react-native-snap-carousel](https://github.com/archriss/react-native-snap-carousel)
* [react-native-splash-screen](https://github.com/crazycodeboy/react-native-splash-screen)
* [react-native-storage](https://github.com/sunnylqm/react-native-storage)
* [react-native-swipeout](https://github.com/dancormier/react-native-swipeout)
* [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons)
* [rn-placeholder](https://github.com/mfrachet/rn-placeholder)
* [react-native-router-flux](https://github.com/aksonov/react-native-router-flux)
* [react-native-swiper](https://github.com/leecade/react-native-swiper)
* [react-native-swipeout](https://github.com/dancormier/react-native-swipeout)      // 特效按钮
* [tcomb-form-native](https://github.com/gcanti/tcomb-form-native)
* [react-native-circular-progress](https://github.com/bgryszko/react-native-circular-progress)   //进度环
* [react-native-storage](https://github.com/sunnylqm/react-native-storage)
* [react-native-easy-toast](https://github.com/crazycodeboy/react-native-easy-toast)     // 错误信息提示框
* [react-native-qrcode](https://github.com/cssivision/react-native-qrcode)      // 二维码
* [react-native-radio-buttons](https://github.com/ArnaudRinquin/react-native-radio-buttons)
* [react-native-simple-radio-button](https://github.com/moschan/react-native-simple-radio-button)
* [react-native-parallax-scroll-view](https://github.com/i6mi6/react-native-parallax-scroll-view)
* [react-native-tab-view](https://github.com/react-native-community/react-native-tab-view)      // tabView 吸顶
* [react-native-device-info](https://github.com/rebeccahughes/react-native-device-info)     // 获取设备识别码
* [react-native-fetch-blob](https://github.com/wkh237/react-native-fetch-blob)      // 下载上传组件
* [react-native-pdf](https://github.com/wonday/react-native-pdf)
* [react-native-fs](https://github.com/itinance/react-native-fs)        // 文件上传下载
* [react-native-fast-image](https://github.com/DylanVann/react-native-fast-image)
* [react-native-img-cache](https://github.com/wcandillon/react-native-img-cache)    // 图片缓存
* [path-to-regexp](https://github.com/pillarjs/path-to-regexp)      // Turn a path string such as /user/:name into a regular expression.
* [react-native-blur](https://github.com/react-native-community/react-native-blur)      // react native 毛玻璃



#### 报错处理

* error: bundling failed: Error: While resolving module `react-native-vector-icons/Ionicons`, the Haste package `react-native-vector-icons` was found. However the module `Ionicons` could not be found within the package. Indeed, none of these files exist:
方法: rm ./node_modules/react-native/local-cli/core/__fixtures__/files/package.json

