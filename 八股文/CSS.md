# CSS

## 一、盒子模型

### 1.1 什么是盒子模型

- ​	展示在页面上的每一个元素都可以视为一个盒模型，且本质上都是一个矩形盒子，盒模型总共分为两种，分别为：`W3C标准盒模型`、`IE盒模型`。
- 之所以分为两类，是因为二者在性质上有些不同，具体来说是在**计算宽高时**的差异。
- 页面中的矩形盒子宽高可以通过`width`、`heitht`属性进行配置，但这只是设置了**content**内容部分的宽高，除此之外，盒子宽高还受`padding`、`border`两个属性的影响。
- `W3C标准盒模型`、`IE盒模型`两种盒子类型，也就是因为对`padding`、`border`两个属性处理的形式不同，从而划分为两类的。



### 1.2 W3C标准盒模型

- `W3C标准盒模型`在计算盒子宽高时，为`width/height`、`padding`、`border`四个属性相加。

  实际也就是 `content+padding+border`，因为我们通过css设置`width/height`属性就是用来定义盒子**内容**宽高的。

  实际操作：

  ![W3CBox1.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5047b7d4f544c66943feb5a10f53d6c~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

- 

- ```html
  <style>
    article {
      width: 100%;
      background-color: #16a085;
    }
    .box {
      width: 100px;
      height: 100px;
      padding: 10px;
      border: 10px solid black;
      margin: 5px;
      
      background-color: #f39c12;
      color: white;
    }
  </style>
  
  <body>
    <article>
      <div class="box W3CBox">
        <span>W3CBox</span>
      </div>
    </article>
  </body>
  复制代码
  ```

  通过控制台工具，看看盒子宽高到底是不是上文讲的那样： `content+padding+border`

  ![W3CBox2.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8769b9f16f3e4fb38b6de367ae7d1ecf~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

  现在来分析一下：

  ![W3CBox3.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4d194f417a394d56bce4de2f503f8d45~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

  1. **content**：content= width/height =  100 * 100 (px)
  2. padding：在content的基础上设置padding为10px
     - 此时盒子宽度 width+paddingLeft/Right =100 + 20 (px)
     - 此时盒子高度 width+paddingTop/Bottom =100 + 20 (px)
  3. border：在上面的基础上又对盒子宽高产生了影响
     - 此时盒子宽度 width+paddingLeft/Right + borderLeft/Right =100 + 20 + 20(px)
     - 此时盒子高度 width+paddingTop/Bottom + borderTop/Bottom =100 + 20 + 20(px)

  整体计算下来，盒子宽高为 `140 *140 (px)`，完全符合上述结论。







### 1.3 IE盒模型

- `IE盒模型`在计算盒子宽高时，只包含`width/height`即内容**content**部分。但值得注意的是，这content包含了`padding`、`border`两个属性。

  也即是说，盒子最终的宽高，就是我们设置的`width/height`,若我们又设置了`padding`、`border`两个属性，则这两个属性不会对宽高造成影响，而是包含在了设置的`width/height`中。

- 实际操作：

  ![IEBox1.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/87cc46cd70e4474f9a818fec54878878~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

  ```css
  .IEBox { box-sizing: border-box; }
  复制代码
  <div class="box IEBox"> <span>IEBox</span> </div>
  复制代码
  ```

  可以发现，其他配置相同，只是改变了盒子模型，但尺寸却差了好多，我们通过控制台工具，看看盒子宽高到底为多少？

  ![IEBox2.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/04e7828f1a464fccb67e30d2ab2c987c~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

  ![IEBox3.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/93cd12c7b098432b9779b1c303a7a525~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

  通过结果来看，完全符合上述结论，盒子的尺寸只包含**content**部分，也就是我们定义的 `width/height`属性。之后定义的`padding`、`border`都会被包含到**content**中。





### 1.4 box-sizing

- 上面两个例子已经让我们清晰的发现两种盒模型的差异，那我们怎么切换两种盒子模型呢，其实上文已经使用过了，也就是`box-sizing`属性。

- **语法**

  ```rust
  box-sizing: content-box|border-box|inherit;
  ```

  - content-box
    - 默认值，设置为W3C标准盒模型
  - border-box
    - 设置为IE盒模型
  - inherit
    - 继承父级盒子类型







## 二、BFC

### 2.1 BFC到底是什么东西

- `BFC` 全称：`Block Formatting Context`， 名为 "块级格式化上下文"。
- `W3C`官方解释为：`BFC`它决定了元素如何对其内容进行定位，以及与其它元素的关系和相互作用，当涉及到可视化布局时，`Block Formatting Context`提供了一个环境，`HTML`在这个环境中按照一定的规则进行布局。
- 简单来说就是，`BFC`是一个完全独立的空间（布局环境），让空间里的子元素不会影响到外面的布局。那么怎么使用`BFC`呢，`BFC`可以看做是一个`CSS`元素属性





### 2.2 触发BFC

- 根元素
- `position: absolute/fixed`
- `display: inline-block / table`
- `float` 元素
- `ovevflow !== visible`
- `overflow: hidden`
- `display: flex`





### 2.3 BFC的规则

- `BFC`就是一个块级元素，块级元素会在垂直方向一个接一个的排列
- 属于同一个 `BFC` 的两个相邻 `Box` 的 `margin` 会发生重叠
- `BFC` 中子元素的 `margin box` 的左边， 与包含块 (BFC) `border box`的左边相接触 (子元素 `absolute` 除外)
- `BFC` 的区域不会与 `float` 的元素区域重叠
- 计算 `BFC` 的高度时，浮动子元素也参与计算
- 文字层不会被浮动层覆盖，环绕于周围





### 2.4 BFC解决了什么问题

#### 2.4.1 使用Float脱离文档流，高度塌陷

- ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>高度塌陷</title>
      <style>
          .box {
              margin: 100px;
              width: 100px;
              height: 100px;
              background: red;
              float: left;
          }
          .container {
              background: #000;
          }
      </style>
  </head>
  <body>
      <div class="container">
          <div class="box"></div>
          <div class="box"></div>
      </div>
  </body>
  </html>
  复制代码
  ```

  **效果：**

  ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d86802070eda44a5a3c84c8ae1469e12~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

  - 可以看到上面效果给`box`设置完`float`结果脱离文档流，使`container`高度没有被撑开，从而背景颜色没有颜色出来，解决此问题可以给`container`触发`BFC`，上面我们所说到的触发`BFC`属性都可以设置。

- **修改代码**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>高度塌陷</title>
      <style>
          .box {
              margin: 100px;
              width: 100px;
              height: 100px;
              background: red;
              float: left;
          }
          .container {
              background: #000;
              display: inline-block;
          }
      </style>
  </head>
  <body>
      <div class="container">
          <div class="box"></div>
          <div class="box"></div>
      </div>
  </body>
  </html>
  复制代码
  ```

  **效果：**

  ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7d6c78771f5b4fbca1aad10797cd9777~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)





#### 2.4.2 Margin边距重叠

- ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
      <style>
          .box {
              margin: 10px;
              width: 100px;
              height: 100px;
              background: #000;
          }
      </style>
  </head>
  <body>
      <div class="container">
          <div class="box"></div>
          <div class="box"></div>
      </div>
  </body>
  </html>
  复制代码
  ```

  **效果：**

  ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9205525816724ed792539197e22ac098~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

  - 可以看到上面我们为两个盒子的`margin`外边距设置的是`10px`，可结果显示两个盒子之间只有`10px`的距离，这就导致了`margin`塌陷问题，这时`margin`边距的结果为最大值，而不是合，为了解决此问题可以使用`BFC`规则（为元素包裹一个盒子形成一个完全独立的空间，做到里面元素不受外面布局影响），或者简单粗暴方法一个设置`margin`，一个设置`padding`。

- **修改代码**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Margin边距重叠</title>
      <style>
          .box {
              margin: 10px;
              width: 100px;
              height: 100px;
              background: #000;
          }
      </style>
  </head>
  <body>
      <div class="container">
          <div class="box"></div>
          <p><div class="box"></div></p>
      </div>
  </body>
  </html>
  ```

  **效果：**

  ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/de9617862fc14c07b4c94d14c21760c8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)





#### 2.4.3 两栏布局

- ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>两栏布局</title>
      <style>
              div {
                   width: 200px;
                   height: 100px;
                   border: 1px solid red;
              }
  
      </style>
  </head>
  <body>
      <div style="float: left;">
          两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局
      </div>
      <div style="width: 300px;">
          我是蛙人，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭
      </div>
  </body>
  </html>
  复制代码
  ```

  **效果：**

  ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8224273e52044717820a99cf471b8527~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

  - 可以看到上面元素，第二个`div`元素为`300px`宽度，但是被第一个`div`元素设置`Float`脱离文档流给覆盖上去了，解决此方法我们可以把第二个`div`元素设置为一个`BFC`。

- **修改代码**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>两栏布局</title>
      <style>
              div {
                   width: 200px;
                   height: 100px;
                   border: 1px solid red;
              }
  
      </style>
  </head>
  <body>
      <div style="float: left;">
          两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局两栏布局
      </div>
      <div style="width: 300px;display:flex;">
          我是蛙人，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭，如有帮助请点个赞叭
      </div>
  </body>
  </html>
  复制代码
  ```

  **效果：**

  ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e1d259e81b5f4868a8d37570cc523121~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)











## 三、层叠上下文

### 3.1 **层叠等级：层叠上下文在z轴上的排序**

- 在同一层叠上下文中，层叠等级才有意义
- `z-index`的优先级最高

![img](https://s.poetries.work/gitee/2020/09/111.png)



### 3.2 什么是“层叠上下文”

- 层叠上下文(stacking context)，是HTML中一个三维的概念。在CSS2.1规范中，每个盒模型的位置是三维的，分别是平面画布上的`X轴`，`Y轴`以及表示层叠的`Z轴`。一般情况下，元素在页面上沿`X轴Y轴`平铺，我们察觉不到它们在`Z轴`上的层叠关系。而一旦元素发生堆叠，这时就能发现某个元素可能覆盖了另一个元素或者被另一个元素覆盖。

  如果一个元素含有层叠上下文，(也就是说它是层叠上下文元素)，我们可以理解为这个元素在`Z轴`上就“高人一等”，最终表现就是它离屏幕观察者更近。

  > **具象的比喻**：你可以把层叠上下文元素理解为理解为**该元素当了官**，而其他非层叠上下文元素则可以理解为普通群众。凡是“当了官的元素”就比普通元素等级要高，也就是说元素在`Z轴`上更靠上，更靠近观察者。





### 3.3 什么是“层叠等级”

- 那么，层叠等级指的又是什么？层叠等级(stacking level，叫“层叠级别”/“层叠水平”也行)

  - 在同一个层叠上下文中，它描述定义的是该层叠上下文中的层叠上下文元素在`Z轴`上的上下顺序。
  - 在其他普通元素中，它描述定义的是这些普通元素在`Z轴`上的上下顺序。

  说到这，可能很多人疑问了，不论在层叠上下文中还是在普通元素中，层叠等级都表示元素在`Z轴`上的上下顺序，那就直接说它描述定义了所有元素在`Z轴`上的上下顺序就OK啊！为什么要分开描述？

  为了说明原因，先举个栗子：

  > **具象的比喻**：我们之前说到，处于层叠上下文中的元素，就像是元素当了官，等级自然比普通元素高。再想象一下，假设一个官员A是个省级领导，他下属有一个秘书a-1，家里有一个保姆a-2。另一个官员B是一个县级领导，他下属有一个秘书b-1，家里有一个保姆b-2。a-1和b-1虽然都是秘书，但是你想一个省级领导的秘书和一个县级领导的秘书之间有可比性么？甚至保姆a-2都要比秘书b-1的等级高得多。谁大谁小，谁高谁低一目了然，所以根本没有比较的意义。只有在A下属的a-1、a-2以及B下属的b-1、b-2中相互比较大小高低才有意义。

  **再类比回“层叠上下文”和“层叠等级”，就得出一个结论：**

  1. **普通元素的层叠等级优先由其所在的层叠上下文决定。**
  2. **层叠等级的比较只有在当前层叠上下文元素中才有意义**。不同层叠上下文中比较层叠等级是没有意义的。





### 3.4 如何产生“层叠上下文”

- 其实，层叠上下文也基本上是有一些特定的CSS属性创建的，一般有3种方法：

  - 根层叠上下文(`html`)
  - `position`
  - `css3`属性
    - 父元素的display属性值为`flex|inline-flex`，子元素`z-index`属性值不为`auto`的时候，子元素为层叠上下文元素；
    - 元素的`opacity`属性值不是`1`；
    - 元素的`transform`属性值不是`none`；
    - 元素`mix-blend-mode`属性值不是`normal`；
    - 元素的`filter`属性值不是`none`；
    - 元素的`isolation`属性值是`isolate`；
    - `will-change`指定的属性值为上面任意一个；
    - 元素的`-webkit-overflow-scrolling`属性值设置为`touch`。

- 至此，终于可以上代码了，我们用代码说话，来验证上面的结论：

- **例子1:** **有两个div，p.a、p.b被包裹在一个div里，p.c被包裹在另一个盒子里，只为.a、.b、.c设置`position`和`z-index`属性**

  ```xml
  <style>
    div {  
      position: relative;  
      width: 100px;  
      height: 100px;  
    }  
    p {  
      position: absolute;  
      font-size: 20px;  
      width: 100px;  
      height: 100px;  
    }  
    .a {  
      background-color: blue;  
      z-index: 1;  
    }  
    .b {  
      background-color: green;  
      z-index: 2;  
      top: 20px;  
      left: 20px;  
    }  
    .c {  
      background-color: red;  
      z-index: 3;  
      top: -20px;  
      left: 40px;  
    }
  </style>
  
  <body>  
    <div>  
      <p class="a">a</p>  
      <p class="b">b</p>  
    </div> 
  
    <div>  
      <p class="c">c</p>  
    </div>  
  </body> 
  复制代码
  ```

  效果：

  

  ![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/8/30/165890e9dcadf485~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp)

  - 因为p.a、p.b、p.c三个的父元素div都没有设置`z-index`，所以不会产生层叠上下文，所以.a、.b、.c都处于由`<html></html>`标签产生的“根层叠上下文”中，属于同一个层叠上下文，此时谁的`z-index`值大，谁在上面。

- **例子2：** **有两个div，p.a、p.b被包裹在一个div里，p.c被包裹在另一个盒子里，同时为两个div和.a、.b、.c设置`position`和`z-index`属性**

  ```xml
  <style>
    div {
      width: 100px;
      height: 100px;
      position: relative;
    }
    .box1 {
      z-index: 2;
    }
    .box2 {
      z-index: 1;
    }
    p {
      position: absolute;
      font-size: 20px;
      width: 100px;
      height: 100px;
    }
    .a {
      background-color: blue;
      z-index: 100;
    }
    .b {
      background-color: green;
      top: 20px;
      left: 20px;
      z-index: 200;
    }
    .c {
      background-color: red;
      top: -20px;
      left: 40px;
      z-index: 9999;
    }
  </style>
  
  <body>
    <div class="box1">
      <p class="a">a</p>
      <p class="b">b</p>
    </div>
  
    <div class="box2">
      <p class="c">c</p>
    </div>
  </body>
  复制代码
  ```

  效果：

  

  ![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/8/30/165890ecb78125b2~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp)

  - 我们发现，虽然`p.c`元素的`z-index`值为9999，远大于`p.a`和`p.b`的`z-index`值，但是由于`p.a`、`p.b`的父元素`div.box1`产生的层叠上下文的`z-index`的值为2，`p.c`的父元素`div.box2`所产生的层叠上下文的`z-index`值为1，所以`p.c`永远在`p.a`和`p.b`下面。
  - 同时，如果我们只更改`p.a`和`p.b`的`z-index`值，由于这两个元素都在父元素`div.box1`产生的层叠上下文中，所以，谁的`z-index`值大，谁在上面。





### 3.5 什么是“层叠顺序”

- 说完“层叠上下文”和“层叠等级”，我们再来说说“层叠顺序”。“层叠顺序”(stacking order)表示元素发生层叠时按照特定的顺序规则在`Z轴`上垂直显示。**由此可见，前面所说的“层叠上下文”和“层叠等级”是一种概念，而这里的“层叠顺序”是一种规则。**

  ![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/8/30/1658910c5cb364b6~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp)

  在不考虑CSS3的情况下，当元素发生层叠时，层叠顺讯遵循上面途中的规则。 **这里值得注意的是：**

  

  1. 左上角"层叠上下文`background/border`"指的是层叠上下文元素的背景和边框。
  2. `inline/inline-block`元素的层叠顺序要高于`block`(块级)/`float`(浮动)元素。
  3. 单纯考虑层叠顺序，`z-index: auto`和`z-index: 0`在同一层级，但这两个属性值本身是有根本区别的。

  > 对于上面第2条，为什么`inline/inline-block`元素的层叠顺序要高于`block`(块级)/`float`(浮动)元素？这个大家可以思考一下！ 其实很简单，像`border/background`属于装饰元素的属性，浮动和块级元素一般用来页面布局，而网页设计之初最重要的就是文字内容，所以在发生层叠时会优先显示文字内容，保证其不被覆盖。



### 3.6 总结规则

- 首先先看要比较的两个元素是否处于同一个层叠上下文中
  - 如果是，谁的层叠等级大，谁在上面（怎么判断层叠等级大小呢？——看“层叠顺序”图）。 
  - 如果两个元素不在统一层叠上下文中，请先比较他们所处的层叠上下文的层叠等级。
-  当两个元素层叠等级相同、层叠顺序相同时，在DOM结构中后面的元素层叠等级在前面元素之上。









## 四、左右居中方案

- 行内元素: `text-align: center`

  - ```css
    /* 方案1 */
    .wrap {
      text-align: center
    }
    .center {
      display: inline;
      /* or */
      /* display: inline-block; */
    }
    ```

- 定宽块状元素: 左右 `margin` 值为 `auto`

  - ```css
    /* 方案2 */
    .center {
      width: 100px;
      margin: 0 auto;
    }
    ```

- 不定宽块状元素: `table`布局，`position + transform`

  - ```css
    /* 方案3 */
    .wrap {
      position: relative;
    }
    .center {
      position: absulote;
      left: 50%;
      transform: translateX(-50%);
    }
    ```





## 五、上下垂直居中方案

- 定高：`margin`，`position + margin`(负值)

  - ```css
    /* 定高方案1 */
    .center {
      height: 100px;
      margin: 50px 0;   
    }
    /* 定高方案2 */
    .center {
      height: 100px;
      position: absolute;
      top: 50%;
      margin-top: -25px;
    }
    ```

- 不定高：`position` + `transform`，`flex`，`IFC + vertical-align:middle`

  - ```css
    /* 不定高方案1 */
    .center {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
    }
    /* 不定高方案2 */
    .wrap {
      display: flex;
      align-items: center;
    }
    .center {
      width: 100%;
    }
    /* 不定高方案3 */
    /* 设置 inline-block 则会在外层产生 IFC，高度设为 100% 撑开 wrap 的高度 */
    .wrap::before {
      content: '';
      height: 100%;
      display: inline-block;
      vertical-align: middle;
    }
    .wrap {
      text-align: center;
    }
    .center {
      display: inline-block;  
      vertical-align: middle;
    }
    ```





## 六、选择器权重计算方式

- !important > 内联样式 = 外联样式 > ID选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 元素选择器 = 伪元素选择器 > 通配选择器 = 后代选择器 = 兄弟选择器
  1. 属性后面加`!import`会覆盖页面内任何位置定义的元素样式
  2. 作为`style`属性写在元素内的样式
  3. `id`选择器
  4. 类选择器
  5. 标签选择器
  6. 通配符选择器（`*`）
  7. 浏览器自定义或继承
  8. **同一级别：后写的会覆盖先写的**
- 计算方式
  - 用1表示派生选择器的优先级
  - 用10表示类选择器的优先级
  - 用100标示ID选择器的优先级
    - div.test1 .span var 优先级 1+10 +10 +1
    - span#xxx .songs li 优先级1+100 + 10 + 1
    - \#xxx li 优先级 100 +1

- 案例

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/de28690d95484b0c9c1345e65f4e24c4~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9ab8d24f4ca9491987859bc2163ab679~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)





## 七、清除浮动

- 在浮动元素后面添加 `clear:both`的空 `div` 元素

  - ```html
    <div class="container">
        <div class="left"></div>
        <div class="right"></div>
        <div style="clear:both"></div>
    </div>
    ```
- 给父元素添加 `overflow:hidden` 或者 `auto` 样式，触发`BFC`

  - ```html
    <div class="container">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    ```

    ```css
    .container{
        width: 300px;
        background-color: #aaa;
        overflow:hidden;
        zoom:1;   /*IE6*/
    }
    ```
- 使用伪元素，也是在元素末尾添加一个点并带有 `clear: both` 属性的元素实现的。

  - ```html
    <div class="container clearfix">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    ```

    ```css
    .clearfix{
        zoom: 1; /*IE6*/
    }
    .clearfix:after{
        content: ".";
        height: 0;
        clear: both;
        display: block;
        visibility: hidden;
    }
    ```









## 八、左边定宽，右边自适应方案

### 8.1 float + margin，

- **思路**

  - **两列**：使用 `float` ，由于脱离文档流，所以元素在同一行上
  - **定宽**：左边设置固定宽度即可，右边通过 `margin-left` 从形式上隔离二者的重叠

- 实现：

  ```css
  .left {
      float: left;
      width: 100px;
  }
  .right {
      margin-left: 120px;
  }
  ```





### 8.2 float + calc

- **思路**

  - **两列**：使用 `float` 使两个 `div` 处于同一行中
  - **定宽**：左边设置固定宽度即可，右边根据计算来确定宽度以实现自适应
- 实现：

  ```css
  .left {
      width: 100px;
      float: left;
  }
  .right {
      float: left;
      width: calc(100% - 120px);
      margin-left: 20px;
  }
  ```





### 8.3 inline-block + calc

- **思路**

  - **两列**：使用 `inline-block` 使两个 `div` 处于同一行中
  - **定宽**：左边设置固定宽度即可，右边根据计算来确定宽度以实现自适应
- **实现**

  - ```html
    <div class="wrapper">
      <div class="left">
        左边
      </div>
      <div class="right">
        右边
      </div>
    </div>
    ```
  - ```css
    .left {
        width: 100px;
    }
    .right {
        width: calc(100% - 100px);
        margin-left: 20px;
    }
    ```





### 8.4 position + calc

- **思路**

  - **两列**：使用 `absolute` 使两个 `div` 处于同一行中
  - **定宽**：左边设置固定宽度即可，右边根据计算来确定宽度以实现自适应
- **实现**

  - ```css
    .parent {
        position: relative;
        .left {
            position: absolute;
            left: 0;
            width: 100px;
        }
        .right {
            position: absolute;
            left: 120px;
            width: calc(100% - 120px);
        }
    }
    ```





### 8.5 float + BFC

- **思路**

  - **两列**：使用 `float` 和 `overflow: hidden` 利用 BFC 的特性使元素在一行上
  - **定宽**：左边设置固定宽度即可，右边由于 BFC 宽度会自适应
- **实现**

  - ```css
    .left {
        float: left;
        width: 100px;
        margin-right: 20px;
    }
    .right {
        overflow: hidden;
    }
    ```







### 8.6 flex

- **思路**

  - **两列**：默认 flex 布局元素在水平方向上
  - **定宽**：左边设置固定宽度即可，右边利用 `flex: 1` 会将剩余空间占据实现宽度自适应。
- **实现**

  - ```css
    .parent {
        display: flex;
        .left {
            width: 100px;
            margin-right: 20px;
        }
        .right {
            flex: 1;
        }
    }
    ```











## 九、左右两边定宽，中间自适应

### 9.1 圣杯布局

- 圣杯布局是一种**三列**，**左右栏固定**，**中间栏边框自适应**的网页布局
- 特点：

  - 采用float浮动
  - 三列布局，中间宽度自适应，两边定宽；
  - 中间元素要在html结构中处于开头；
- **实现：**

  - ```html
    <div class="container">
        <!-- mian要放开头 -->
        <div class="main">main</div>
        <div class="left">left</div>
        <div class="right">right</div>
      </div>
    
      <style>
        .container {
          min-width: 600px;
          overflow: hidden;
          padding: 0 200px;
          box-sizing: border-box;
        }
    
        .left,
        .main,
        .right {
          float: left;
          min-height: 100px;
        }
    
        .left {
          position: relative;
          left: -200px;
          margin-left: -100%;
          width: 200px;
          background: green;
        }
    
        .main {
          width: 100%;
          background: blue;
        }
    
        .right {
          position: relative;
          right: -200px;
          margin-left: -200px;
          width: 200px;
          background: red;
        }
      </style>
    ```
- **解析**：

  - 这里的`.main`使用了`width:100%`，会导致`.left`和`.right`会换行，所以`.left`的`margin-left:100%`（**margin-left百分比相对的是父元素**），`.right`的`margin-left: -200px`（**200px刚好是自身的宽度**）实现在同一行
  - `.container`的`padding`刚好与`.left`和`.right`的长度一样，方便`.left`向左方向，`.right`向右方向，相对定位移动自身宽度的长度,避免遮挡到`.main`
  - 然后`.container`需要设置`min-width`，因为`.left`的`margin-left`是`100%`，相对的是**父元素**，如果`.container`宽度变小，则`.main`宽度也会变小，如果小于`.left`的宽度，则不足以和`.main`在一行，`.left`则会换行







### 9.2 双飞翼布局

- 双飞翼布局也是一种**三列**，**左右栏固定**，**中间栏边框自适应**的网页布局
- 解决问题也和圣杯布局类似，也是采用**三栏全部float浮动**，**左右两栏加上负margin**，**跟中间栏div并排**，形成三栏布局。**主要不同是在解决 “中间栏div内容不被遮挡”方式不一样**。
- **实现：**

  - ```html
      <div class="container">
        <div class="main">
          <div class="content">main</div>
        </div>
        <div class="left">left</div>
        <div class="right">right</div>
      </div>
      <style>
        .main {
          width: 100%;
          background: blue;
        }
    
        .content {
          margin: 0 200px;
        }
    
        .left,
        .main,
        .right {
          float: left;
          min-height: 100px;
        }
    
        .left {
          margin-left: -100%;
          width: 200px;
          background: green;
        }
    
        .right {
          margin-left: -200px;
          width: 200px;
          background: red;
        }
      </style>
    ```
- 和圣杯布局主要区别：

  - `.main`内部多了子元素`div.content`
  - 子元素`div.content`里用`margin-left`和`margin-right`，去保证不被`.left`和`.right`遮挡
  - `.left`,`.right`不需要使用定位去避免遮挡`.main`





### 9.3 flex布局

- ```html
    <div class="container">
      <!-- 注意是left main right -->
      <div class="left">left</div>
      <div class="main">main</div>
      <div class="right">right</div>
    </div>
    <style>
      .container {
        display: flex;
        min-height: 100px;
      }
  
      .left {
        flex-basis: 200px;
        background: green;
      }
  
      .main {
        flex: auto;
        background: blue;
      }
  
      .right {
        flex-basis: 200px;
        background: red;
      }
    </style>
  ```
- 解析：

  - **flex布局**，弹性布局，可以简便、完整、响应式地实现各种页面布局，未来的布局的首选方案（当然如果你要考虑ie,另说）
  - `.main`使用`flex`属性，`.left` 和`.right`使用`flex-basis` 属性
  - `flex`属性是`flex-grow`（如果有剩余空间，是否扩大）, `flex-shrink`（如果空间不够，是否压缩） 和 `flex-basis`（本身的空间大小）的简写，默认值为`0 1 auto`。后两个属性可选
  - `flex属性`有两个快捷值：`auto (1 1 auto)` 和 `none (0 0 auto)`,这里使用的是`auto`，如果有剩余空间，则扩大，如果空间不够,则压缩





### 9.4 absolute布局

- `absolute`绝对定位的元素的位置与文档流无关，因此不占据空间。
- 这一点与相对定位不同，相对定位实际上被看作普通流定位模型的一部分。
- ```html
    <div class="container">
    <!-- 注意是left main right -->
      <div class="left">left</div>
      <div class="main">main</div>
      <div class="right">right</div>
    </div>
    <style>
      .container {
        position: relative;
      }
  
      .left,
      .main,
      .right {
        min-height: 100px;
      }
  
      .left {
        position: absolute;
        left: 0;
        top: 0;
        width: 200px;
        background: green;
      }
  
      .main {
        margin: 0 200px 0 200px;
        background: blue;
      }
  
      .right {
        position: absolute;
        right: 0;
        top: 0;
        width: 200px;
        background: red;
      }
    </style>
  ```
- 解析：

  - `.main`使用`margin`避免被`.left`和`.right`遮挡
  - `.left`和`.right`使用`absolute`定位，不占据文档流





### 9.5 float

- ```css
  .left {
    width: 120px;
    float: left;
  }
  .right {
    float: right;
    width: 120px;
  }
  .center {
    margin: 0 120px; 
  }
  ```
- left和right分别左右浮动，center设置margin不遮盖左右







### 9.6 float + calc

- ```css
  .left {
    width: 120px;
    float: left;
  }
  .right {
    float: right;
    width: 120px;
  }
  .center {
    width: calc(100% - 240px);
    margin-left: 120px;
  }
  ```
- left和right左右浮动，center设置margin-left避免遮盖左元素，calc设置宽度不遮盖右元素











## 十、CSS动画和过渡

### 10.1 **animation / keyframes**

- `animation-name`: 动画名称，对应`@keyframes`

- `animation-duration`: 间隔

- `animation-timing-function`: 曲线

- `animation-delay`: 延迟

- `animation-iteration-count`: 次数

  - `infinite`: 循环动画

- `animation-direction`: 方向

  - `alternate`: 反向播放

- `animation-fill-mode`: 静止模式

  - `forwards`: 停止时，保留最后一帧
  - `backwards`: 停止时，回到第一帧
  - `both`: 同时运用 `forwards / backwards`

- 常用钩子: `animationend`

- > 动画属性: 尽量使用动画属性进行动画，能拥有较好的性能表现

  - `translate`
  - `scale`
  - `rotate`
  - `skew`
  - `opacity`
  - `color`







### 10.2 transform

- 位移属性 `translate( x , y )`
- 旋转属性 `rotate()`
- 缩放属性 `scale()`
- 倾斜属性 `skew()`





### 10.3 transition

- `transition-property`（过渡的属性的名称）。
- `transition-duration`（定义过渡效果花费的时间,默认是 `0`）。
- `transition-timing-function:linear(匀速) ease`(慢速开始，然后变快，然后慢速结束)（规定过渡效果的时间曲线，最常用的是这两个）。
- `transition-delay`（规定过渡效果何时开始。默认是 0）

> 般情况下，我们都是写一起的，比如：`transition： width 2s ease 1s`







### 10.4 **关键帧动画animation**

- > 一个关键帧动画，最少包含两部分，animation 属性及属性值（动画的名称和运行方式运行时间等）。@keyframes（规定动画的具体实现过程）

  **animation 属性可以拆分为**

  - `animation-name` 规定@keyframes 动画的名称。
  - `animation-duration` 规定动画完成一个周期所花费的秒或毫秒。默认是 `0`。
  - `animation-timing-function` 规定动画的速度曲线。默认是 “ease”，常用的还有`linear`，同`transtion` 。
  - `animation-delay` 规定动画何时开始。默认是 0。
  - `animation-iteration-count` 规定动画被播放的次数。默认是 1，但我们一般用`infinite`，一直播放

  > 而`@keyframes`的使用方法，可以是`from->to`（等同于0%和100%），也可以是从`0%->100%`之间任意个的分层设置。我们通过下面一个稍微复杂点的`demo`来看一下，基本上用到了上面说到的大部分知识

  ```css
  eg:
     @keyframes mymove
    {
        from {top:0px;}
        to {top:200px;}
    }
   
  /* 等同于： */
   
  @keyframes mymove
  {
   0%   {top:0px;}
   25%  {top:200px;}
   50%  {top:100px;}
   75%  {top:200px;}
   100% {top:0px;}
  }   
  ```

  **用css3动画使一个图片旋转**

  ```css
  #loader {
      display: block;
      position: relative;
      -webkit-animation: spin 2s linear infinite;
      animation: spin 2s linear infinite;
  }
  
  @-webkit-keyframes spin {
      0%   {
          -webkit-transform: rotate(0deg);
          -ms-transform: rotate(0deg);
          transform: rotate(0deg);
      }
      100% {
          -webkit-transform: rotate(360deg);
          -ms-transform: rotate(360deg);
          transform: rotate(360deg);
      }
  }
  
  @keyframes spin {
      0%   {
          -webkit-transform: rotate(0deg);
          -ms-transform: rotate(0deg);
          transform: rotate(0deg);
      }
  
      100% {
          -webkit-transform: rotate(360deg);
          -ms-transform: rotate(360deg);
          transform: rotate(360deg);
      }
  }
  ```









## 十一、CSS3的新特性有哪些

- `transition`：过渡
- `transform`: 旋转、缩放、移动或倾斜
- `animation`: 动画
- `gradient`: 渐变
- `box-shadow`: 阴影
- `border-radius`: 圆角
- `word-break`: `normal|break-all|keep-all`; 文字换行(默认规则|单词也可以换行|只在半角空格或连字符换行)
- `text-overflow`: 文字超出部分处理
- `text-shadow`: 水平阴影，垂直阴影，模糊的距离，以及阴影的颜色。
- `box-sizing`: `content-box|border-box` 盒模型
- 媒体查询 `@media screen and (max-width: 960px) {}`还有打印`print`







## 十二、列举几个css中可继承和不可继承的元素

- 不可继承的：`display、margin、border、padding、background、height、min-height、max-height、width、min-width、max-width、overflow、position、left、right、top、bottom、z-index、float、clear、table-layout、vertical-align`
- 所有元素可继承：`visibility`和`cursor`。
- 内联元素可继承：`letter-spacing、word-spacing、white-space、line-height、color、font、font-family、font-size、font-style、font-variant、font-weight、text-decoration、text-transform、direction`。
- 终端块状元素可继承：`text-indent和text-align`。
- 列表元素可继承：`list-style、list-style-type、list-style-position、list-style-imag`e`。

**transition和animation的区别**

> `Animation`和`transition`大部分属性是相同的，他们都是随时间改变元素的属性值，他们的主要区别是`transition`需要触发一个事件才能改变属性，而`animation`不需要触发任何事件的情况下才会随时间改变属性值，并且`transition`为2帧，从`from .... to`，而`animation`可以一帧一帧的

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