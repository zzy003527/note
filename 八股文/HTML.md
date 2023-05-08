# HTML

## 一、meta标签：自动刷新/跳转

- 假设要实现一个类似 PPT 自动播放的效果，你很可能会想到使用 JavaScript 定时器控制页面跳转来实现。但其实有更加简洁的实现方法，比如通过 meta 标签来实现：

  ```html
  <meta http-equiv="Refresh" content="5; URL=page2.html">
  ```

  上面的代码会在 5s 之后自动跳转到同域下的 page2.html 页面。我们要实现 PPT 自动播放的功能，只需要在每个页面的 meta 标签内设置好下一个页面的地址即可。

- 另一种场景，比如每隔一分钟就需要刷新页面的大屏幕监控，也可以通过 meta 标签来实现，只需去掉后面的 URL 即可：

  ```html
  <meta http-equiv="Refresh" content="60">
  ```

- **meta viewport相关**

  - ```html
    <!DOCTYPE html>  <!--H5标准声明，使用 HTML5 doctype，不区分大小写-->
    <head lang=”en”> <!--标准的 lang 属性写法-->
    <meta charset=’utf-8′>    <!--声明文档使用的字符编码-->
    <meta http-equiv=”X-UA-Compatible” content=”IE=edge,chrome=1″/>   <!--优先使用 IE 最新版本和 Chrome-->
    <meta name=”description” content=”不超过150个字符”/>       <!--页面描述-->
    <meta name=”keywords” content=””/>     <!-- 页面关键词-->
    <meta name=”author” content=”name, email@gmail.com”/>    <!--网页作者-->
    <meta name=”robots” content=”index,follow”/>      <!--搜索引擎抓取-->
    <meta name=”viewport” content=”initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no”> <!--为移动设备添加 viewport-->
    <meta name=”apple-mobile-web-app-title” content=”标题”> <!--iOS 设备 begin-->
    <meta name=”apple-mobile-web-app-capable” content=”yes”/>  <!--添加到主屏后的标题（iOS 6 新增）
    是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏-->
    <meta name=”apple-itunes-app” content=”app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL”>
    <!--添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）-->
    <meta name=”apple-mobile-web-app-status-bar-style” content=”black”/>
    <meta name=”format-detection” content=”telphone=no, email=no”/>  <!--设置苹果工具栏颜色-->
    <meta name=”renderer” content=”webkit”> <!-- 启用360浏览器的极速模式(webkit)-->
    <meta http-equiv=”X-UA-Compatible” content=”IE=edge”>     <!--避免IE使用兼容模式-->
    <meta http-equiv=”Cache-Control” content=”no-siteapp” />    <!--不让百度转码-->
    <meta name=”HandheldFriendly” content=”true”>     <!--针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓-->
    <meta name=”MobileOptimized” content=”320″>   <!--微软的老式浏览器-->
    <meta name=”screen-orientation” content=”portrait”>   <!--uc强制竖屏-->
    <meta name=”x5-orientation” content=”portrait”>    <!--QQ强制竖屏-->
    <meta name=”full-screen” content=”yes”>              <!--UC强制全屏-->
    <meta name=”x5-fullscreen” content=”true”>       <!--QQ强制全屏-->
    <meta name=”browsermode” content=”application”>   <!--UC应用模式-->
    <meta name=”x5-page-mode” content=”app”>   <!-- QQ应用模式-->
    <meta name=”msapplication-tap-highlight” content=”no”>    <!--windows phone 点击无高亮
    设置页面不缓存-->
    <meta http-equiv=”pragma” content=”no-cache”>
    <meta http-equiv=”cache-control” content=”no-cache”>
    <meta http-equiv=”expires” content=”0″>
    ```





## 二、viewport

- ```html
   <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
      // width    设置viewport宽度，为一个正整数，或字符串‘device-width’
      // device-width  设备宽度
      // height   设置viewport高度，一般设置了宽度，会自动解析出高度，可以不用设置
      // initial-scale    默认缩放比例（初始缩放比例），为一个数字，可以带小数
      // minimum-scale    允许用户最小缩放比例，为一个数字，可以带小数
      // maximum-scale    允许用户最大缩放比例，为一个数字，可以带小数
      // user-scalable    是否允许手动缩放
  ```

- 延伸提问

  - 怎样处理 移动端 `1px` 被 渲染成 `2px`问题

    - **局部处理**

      - `meta`标签中的 `viewport`属性 ，`initial-scale` 设置为 `1`
      - `rem`按照设计稿标准走，外加利用`transfrome` 的`scale(0.5)` 缩小一倍即可；

      **全局处理**

      - `mate`标签中的 `viewport`属性 ，`initial-scale` 设置为 `0.5`
      - `rem` 按照设计稿标准走即可





## 三、性能优化

- 性能优化是前端开发中避不开的问题，`性能问题无外乎两方面原因：渲染速度慢、请求时间长`。性能优化虽然涉及很多复杂的原因和解决方案，但其实只要通过合理地使用标签，就可以在一定程度上提升渲染速度以及减少请求时间



### 3.1 **script 标签：调整加载顺序提升渲染速度**

- 由于浏览器的底层运行机制，`渲染引擎在解析 HTML 时，若遇到 script 标签引用文件，则会暂停解析过程`，同时`通知网络线程加载文件，文件加载后会切换至 JavaScript 引擎来执行对应代码`，`代码执行完成之后切换至渲染引擎继续渲染页面`。

- 在这一过程中可以看到，`页面渲染过程中包含了请求文件以及执行文件的时间`，但页面的首次渲染可能并不依赖这些文件，这些请求和执行文件的动作反而延长了用户看到页面的时间，从而降低了用户体验。

- **为了减少这些时间损耗，可以借助 script 标签的 3 个属性来实现。**

  - `async 属性`。立即请求文件，但不阻塞渲染引擎，而是`文件加载完毕后阻塞渲染引擎并立即执行文件内容`
  - `defer 属性`。立即请求文件，但不阻塞渲染引擎，`等到解析完 HTML 之后再执行`文件内容
  - HTML5 标准 type 属性，对应值为“module”。让浏览器按照 ECMA Script 6 标准将文件当作模块进行解析，默认阻塞效果同 defer，也可以配合 async 在请求完成后立即执行。

  ![img](https://s.poetries.work/images/20210421163450.png)

  > 绿色的线表示执行解析 HTML ，蓝色的线表示请求文件，红色的线表示执行文件

  `当渲染引擎解析 HTML 遇到 script 标签引入文件时，会立即进行一次渲染`。所以这也就是为什么构建工具会把编译好的引用 JavaScript 代码的 script 标签放入到 body 标签底部，因为当渲染引擎执行到 body 底部时会先将已解析的内容渲染出来，然后再去请求相应的 JavaScript 文件







### 3.2  **link 标签：通过预处理提升渲染速度**

- 在我们对大型单页应用进行性能优化时，也许会用到按需懒加载的方式，来加载对应的模块，但如果能合理利用 `link` 标签的 `rel` 属性值来进行预加载，就能进一步提升渲染速度。

  - `dns-prefetch`。当 `link` 标签的 `rel` 属性值为“dns-prefetch”时，`浏览器会对某个域名预先进行 DNS 解析并缓存`。这样，当浏览器在请求同域名资源的时候，能省去从域名查询 IP 的过程，从而`减少时间损耗`。下图是淘宝网设置的 DNS 预解析 ![img](https://s.poetries.work/images/20210421163848.png)
  - `preconnect`。让浏览器在一个 HTTP 请求正式发给服务器前预先执行一些操作，这包括`DNS 解析、TLS 协商、TCP 握手`，通过消除往返延迟来为用户节省时间
  - `prefetch/preload`。两个值都是`让浏览器预先下载并缓存某个资源`，但不同的是，`prefetch 可能会在浏览器忙时被忽略`，而 `preload 则是一定会被预先下载`。
  - `prerender`。浏览器不仅会加载资源，还会解析执行页面，进行预渲染

  这几个属性值恰好反映了浏览器获取资源文件的过程，在这里我绘制了一个流程简图，方便你记忆。

  ![img](https://s.poetries.work/images/20210421164758.png)





### 3.3 搜索优化

- meta 标签：提取关键信息
  - 通过 meta 标签可以设置页面的描述信息，从而让搜索引擎更好地展示搜索结果。
  - 示例 `<meta name="description" content="全球最大的中文搜索引擎、致力于让网民更便捷地获取信息，找到所求。百度超过千亿的中文网页数据库，可以瞬间找到相关的搜索结果。">`









## 四、如何高效操作DOM

### 4.1 **为什么说 DOM 操作耗时**

#### 4.1.1 线程切换

- 浏览器为了避免两个引擎同时修改页面而造成渲染结果不一致的情况，增加了另外一个机制，这`两个引擎具有互斥性`，也就是说在某个时刻只有`一个引擎在运行，另一个引擎会被阻塞`。操作系统在进行线程切换的时候需要保存上一个线程执行时的状态信息并读取下一个线程的状态信息，俗称上下文切换。而这个操作相对而言是比较耗时的
- 每次 DOM 操作就会引发线程的上下文切换——从 JavaScript 引擎切换到渲染引擎执行对应操作，然后再切换回 JavaScript 引擎继续执行，这就带来了性能损耗。单次切换消耗的时间是非常少的，但是如果频繁地大量切换，那么就会产生性能问题

比如下面的测试代码，循环读取一百万次 DOM 中的 body 元素的耗时是读取 JSON 对象耗时的 10 倍。

```js
// 测试次数：一百万次
const times = 1000000
// 缓存body元素
console.time('object')
let body = document.body
// 循环赋值对象作为对照参考
for(let i=0;i<times;i++) {
  let tmp = body
}
console.timeEnd('object')// object: 1.77197265625ms

console.time('dom')
// 循环读取body元素引发线程切换
for(let i=0;i<times;i++) {
  let tmp = document.body
}
console.timeEnd('dom')// dom: 18.302001953125ms
 
```







#### 4.1.2 **重新渲染**

- 另一个更加耗时的因素是元素及样式变化引起的再次渲染，在`渲染过程中最耗时的两个步骤为重排（Reflow）与重绘（Repaint）`。
- 浏览器在渲染页面时会将 HTML 和 CSS 分别解析成 DOM 树和 CSSOM 树，然后合并进行排布，再绘制成我们可见的页面。如果在操作 DOM 时涉及到元素、样式的修改，就会引起渲染引擎重新计算样式生成 CSSOM 树，同时还有可能触发对元素的重新排布和重新绘制
- 可能会影响到其他元素排布的操作就会引起重排，继而引发重绘
  - 修改元素边距、大小
  - 添加、删除元素
  - 改变窗口大小
- 引起重绘
  - 设置背景图片
  - 修改字体颜色
  - 改变 `visibility`属性值





### 4.2 **如何高效操作 DOM**

#### 4.2.1 在循环外操作元素

- 比如下面两段测试代码对比了读取 1000 次 JSON 对象以及访问 1000 次 body 元素的耗时差异，相差一个数量级

  ```js
  const times = 10000;
  console.time('switch')
  for (let i = 0; i < times; i++) {
    document.body === 1 ? console.log(1) : void 0;
  }
  console.timeEnd('switch') // 1.873046875ms
  var body = JSON.stringify(document.body)
  console.time('batch')
  for (let i = 0; i < times; i++) {
    body === 1 ? console.log(1) : void 0;
  }
  console.timeEnd('batch') // 0.846923828125ms
   
  ```





#### 4.2.2 批量操作元素

- 比如说要创建 1 万个 div 元素，在循环中直接创建再添加到父元素上耗时会非常多。如果采用字符串拼接的形式，先将 1 万个 div 元素的 html 字符串拼接成一个完整字符串，然后赋值给 `body` 元素的 `innerHTML` 属性就可以明显减少耗时

  ```js
  const times = 10000;
  console.time('createElement')
  for (let i = 0; i < times; i++) {
    const div = document.createElement('div')
    document.body.appendChild(div)
  }
  console.timeEnd('createElement')// 54.964111328125ms
  console.time('innerHTML')
  let html=''
  for (let i = 0; i < times; i++) {
    html+='<div></div>'
  }
  document.body.innerHTML += html // 31.919921875ms
  console.timeEnd('innerHTML')
  ```

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- 

- d