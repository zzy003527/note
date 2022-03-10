# PC网页特效

- 元素偏移量offset系列

  - offset概述

    - offset翻译过来就是偏移量，我们使用offset系列相关属性可以**动态的**得到该元素的位置（偏移）、大小等
    - 获得元素距离带有定位父元素的位置
    - 获得元素自身的大小（宽度高度）
    - 注意：返回的数组都不带单位

  - offset系列常用属性

    | offset系列属性       | 作用                                                         |
    | -------------------- | ------------------------------------------------------------ |
    | element.offsetParent | 返回作为该元素**带有定位**的父级元素，如果父级都没有定位则返回body |
    | element.offsetTop    | 返回元素相对**带有定位**父元素上方的偏移                     |
    | element.offsetLeft   | 返回元素相对**带有定位**父元素左边框的偏移                   |
    | element.offsetWidth  | 返回自身**包括padding、边框、内容区**的宽度，返回数值不带单位（不包含margin） |
    | element.offsetHeight | 返回自身**包括padding、边框、内容区**的高度，返回数值不带单位（不包含margin） |

  - offset和style的区别

    - offset

      - offset可以得到任意样式表中的样式值
      - offset系列获得的数值是没有单位的
      - offsetWidth包含padding+border+width
      - offsetWidth等属性是**只读属性**，只能获取不能赋值
      - **所以，我们想要获取元素大小位置，用offset更合适** 

    - style

      - **style只能得到<u>行内样式表</u>中的样式**
      - style.width获得的是带有单位的字符串
      - style.width获得不包含padding和border的值
      - style.width是**可读写属性**，可以获取也可以赋值
      - **所以，我们想要给元素更改值，则需要用style改变**

      

- 元素可视区client系列

  - client翻译过来就是客户端，我们使用client系列的相关属性来获取元素可视区的相关信息。通过client系列的相关属性可以动态的得到该元素的边框大小、元素大小等。

  - client系列常用属性

    | client系列属性       | 作用                                                         |
    | -------------------- | ------------------------------------------------------------ |
    | element.clientTop    | 返回元素上边框的大小                                         |
    | element.clientLeft   | 返回元素左边框的大小                                         |
    | element.clientWidth  | 返回自身包括padding、内容区的宽度，**不含边框**，返回数值**不带单位** |
    | element.clientHeight | 返回自身包括padding、内容区的高度，**不含边框**，返回数值**不带单位** |

  - 立即执行函数

    - 写法：（function() {})()       或者         (function() {} ());
      - 第二个小括号可以看作是调用函数（内写参数）
    - 主要作用：**创建一个独立的作用域，避免了命名冲突问题**
    - 不需要调用，立马能够自己执行的函数

  - 下面三种情况都会刷新页面都会触发load事件

    - a标签的超链接

    - F5或者刷新按钮（强制刷新）

    - 前进后退按钮

    - 但是在火狐中，有个”往返缓存“，这个缓存中不仅保存着页面数据，还保存了DOM和JavaScript的状态；实际上是将整个页面都保存在了内存里，所以此时后退按钮不能刷新页面

      - 此时可以用pageshow事件来触发。这个事件在页面显示时触发，无论页面是否来自缓存。在重新加载页面中，pageshow会在load事件触发后触发；根据事件对象中的persisted来判断是否是缓存中的页面触发的pageshow事件，**注意这个事件给window添加**

      - ```js
        window.addEventListener('pageshow',function(e) {
         //e.persisted返回的是true，就是说如果这个页面是从缓存取过来的页面，也需要重新计算一下rem的大小
            if(e.persisted) 
        {
            setRemUnit();
        }
        })
        ```

        

- 元素滚动scroll系列

  - scroll翻译过来就是滚动的，我们使用scroll系列的相关属性可以动态的得到该元素的大小、滚动距离等

  - scroll系列常用属性

    | scroll系列属性       | 作用                                           |
    | -------------------- | ---------------------------------------------- |
    | element.scrollTop    | 返回被卷去的上侧距离，返回数值不带单位         |
    | element.scrollLeft   | 返回被卷去的左侧距离，返回数值不带单位         |
    | element.scrollWidth  | 返回自身实际的宽度，不含边框，返回数值不带单位 |
    | element.scrollHeight | 返回自身实际的高度，不含边框，返回数值不带单位 |

    ![QQ图片20211226154737](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211226154737.png)

  - 页面被卷去的头部

    - 如果浏览器的高（或宽）度不足以显示整个页面时，会自动出现滚动条。当滚动条向下滚动时，页面上被隐藏掉的高度，我们就称为页面被卷去的头部。滚动条在滚动时会触发onscroll事件。
    - **页面被卷去的头部：可以通过window.pageYOffset获得，如果是被卷去的左侧用window.pageXOffset**

  - 页面被卷去的头部兼容性解决方案

    - 页面被卷去的头部由兼容性问题，因此被卷去的头部通常有如下几种写法：

      - 声明了DTD，使用document.documentElement.scrollTop**(声明DTD，即有<!DOCTYPE html>)**
      - 未声明DTD，使用document.body.scrollTop
      - 新方法window.pageYOffset和window.pageXOffset,IE9开始支持

    - ```js
      function getScroll() {
      return {
            left:window.pageXOffset || document.documentElement.scrollLeft || document.body.scrollLeft || 0,
             top:window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop ||0
      };
      }
      使用的时候：getScroll().left
      ```

      

- 三大系列总结

  - | 三大系列大小对比    | 作用                                                         |
    | ------------------- | ------------------------------------------------------------ |
    | element.offsetWidth | 返回自身包括padding、边框、内容区的高度，返回数值不带单位    |
    | element.clientWidth | 返回自身包括padding、内容区的高度，不含边框，返回数值不带单位 |
    | element.scrollWidth | 返回自身实际的宽度，不含边框，返回数值不带单位               |

    ![QQ图片20211226165728](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211226165728.png)

  - 它们的主要用法：

    - offset系列常用于获得元素位置：**offsetLeft  、offsetTop**
    - client系列常用于获取元素大小：**clientWidth、clientHeight**
    - scroll经常用于获取滚动距离：**scrollTop、scrollLeft**
    - **注意<u>页面</u>滚动的距离通过**window.pageYOffset和window.pageXOffset 获得

    

- mouseenter和mouseover的区别

  - mouseenter鼠标事件

    - 当鼠标移动到元素上时就会触发mouseenter事件
    - 类似mouseover，它们两者之间的差别是：mouseover鼠标经过自身盒子会触发，经过子盒子还会触发。mouseenter只会经过自身盒子触发
    - 之所以这样，就是因为mouseenter不会冒泡
    - 跟mouseenter搭配鼠标离开的mouseleave同样不会冒泡

    

- 动画函数封装

  - 动画实现原理

    - 核心原理：通过定时器setInterval（）不断移动盒子位置

    - 实现步骤：
      - 获得盒子当前位置
      - 让盒子在当前位置加上1个移动距离
      - 利用定时器不断重复这个操作
      - 加一个结束定时器的条件
      - 注意此元素需要添加定位，才能使用element.style.left

  - 动画函数简单封装

    - 注意函数需要传递2个参数，**动画对象**和**移动到的距离**

    - ```js
      <div></div>
          <span></span>
          <script>
              //简单动画函数封装：obj目标对象、target目标位置
              function animate(obj,target) {
                  var timer = setInterval(function(){
                  if(obj.offsetLeft >= target) {
                      //停止动画，本质是停止定时器
                      clearInterval(timer);
                  }
                  obj.style.left = obj.offsetLeft + 5 + 'px';
              },30)
              }
              var div = document.querySelector('div');
              var span = document.querySelector('span');
              //调用函数
              animate(div,300);
              animate(span,200);
          </script>
      ```

      

  - 动画函数给不同元素记录不同定时器

    - 如果多个元素都使用这个动画函数，每次都要var声明定时器。我们可以给不同的元素使用不同的定时器（自己专门用自己的定时器）

    - 核心原理：利用JS是一门动态语言，可以很方便的给当前对象添加属性

    - ```js
       function animate(obj,target) {
                  obj.timer = setInterval(function(){
                  if(obj.offsetLeft >= target) {
                      //停止动画，本质是停止定时器
                      clearInterval(obj.timer);
                  }
                  obj.style.left = obj.offsetLeft + 1 + 'px';
              },30)
              }
      ```

      obj.timer是传入函数独有的定时器

      

  - 缓动效果原理

    - 缓动动画就是让元素运动速度有所变化，最常见的是让速度慢慢停下来
    - 思路：
      - 让盒子每次移动的距离慢慢变小，速度就会慢慢落下来
      - 核心算法：**（目标值 - 现在的位置） / 10**        （作为每次移动的距离步长）
      - 停止的条件是：让当前盒子位置等于目标位置就停止定时器
      - 步长值需要取整：step = step > 0 ? Math.ceil(step) : Math.floor(step);
    - 动画函数在多个目标值之间移动
      - 当我们点击按钮时，判断步长是正值还是负值
        - 如果是正值，则步长往大了取整
        - 如果是负值，则步长往小了取整

  - 动画函数添加回调函数

    - **回调函数原理**：函数可以作为一个参数。将这个函数作为参数传到另一个函数里面，当那个函数执行完之后，再执行传进去的这个函数，这个过程就叫做**回调**。

    - ```js
      <button class="btn500">点击夏雨荷走500</button>
          <button class="btn800">点击夏雨荷走800</button>
          <div></div>
          <span>夏雨荷</span>
          <script>
              //简单动画函数封装：obj目标对象、target目标位置
              //给不同的元素制定了不同的定时器
              function animate(obj,target,callback) {
                  //当我们不断点击按钮，这个元素的速度会越来越快，因为开启了太多的定时器
                  //解决方案就是让我们元素只有一个定时器执行
                  //先清除以前的定时器，只保留当前的定时器
                  clearInterval(obj.timer);
                  obj.timer = setInterval(function(){
                      //步长值写到定时器里面
                      var step = (target - obj.offsetLeft) / 10;
                      step = step > 0 ? Math.ceil(step) : Math.floor(step);
                  if(obj.offsetLeft == target) {
                      //停止动画，本质是停止定时器
                      clearInterval(obj.timer);
                      //回调函数写到定时器结束里面
                      if(callback) {
                          callback();
                      }
                  }
                  //把每次加1的步长值换为一个慢慢变小的值
                  obj.style.left = obj.offsetLeft + step + 'px';
              },15)
              }
              var span = document.querySelector('span');
              var btn500 = document.querySelector('.btn500');
              var btn800 = document.querySelector('.btn800');
              //调用函数
             btn500.addEventListener('click',function() {
              animate(span,500,function() {});
             })
             btn800.addEventListener('click',function() {
              animate(span,800,function() {
                  span.style.backgroundColor = 'red';
              });
             })
          </script>
      ```

  - 动画函数封装到单独JS文件里面

    - 因为以后经常使用这个动画函数，可以单独封装到一个Js文件里面，使用的时候引用这个JS文件即可。

    - 步骤：

      - 单独新建一个JS文件

      - <script src="js文件名"></script>

  - 网页轮播图

    - 轮播图也称为焦点图，是网页中比较常见的网页特效

    - 功能需求：

      - 鼠标经过轮播图模块，左右按钮显示，离开隐藏左右按钮

        ```js
         //鼠标经过focus就显示隐藏左右按钮
            focus.addEventListener('mouseenter',function() {
                arrow_l.style.display = 'block';
                arrow_r.style.display = 'block';
            })
            focus.addEventListener('mouseleave',function() {
                arrow_l.style.display = 'none';
                arrow_r.style.display = 'none';
            })
        ```

      - 点击右侧按钮一次，图片往左播放一张，一次类推，左侧按钮同理

        - 声明一个变量num，点击一次，自增1，让这个变量乘以图片宽度，就是ul的滚动距离
        - 图片无缝滚动原理
        - 把ul第一个li复制一份，放到ul的最后面（克隆第一张图片）
          - 克隆ul第一个li ，cloneNode()，加true深克隆复制里面的子节点，false浅克隆
        - 当图片滚动到克隆的最后一张图片时，让ul快速的、不做动画的跳到最左侧：left为0
        - 同时num赋值为0

      - 图片播放的同时，下面小圆圈模块跟随一起变化

        - 最简单的做法是再声明一个变量circle，每次点击自增1，注意，左侧按钮也需要这个变量，因此要声明全局变量
        - 但是图片比小圆圈多一个，必须加一个判断条件——如果circle == 圆圈数，就复原为0

      - 点击小圆圈，可以播放相应图片

        - 引入js文件时，index.js依赖animate.js，所以animate.js要写在index.js上面
        - 使用动画函数前提：该元素有定位
        - 注意是ul移动而不是li移动
        - 滚动图片核心算法：点击某个小圆圈，就让图片滚动，小圆圈的索引号乘以图片的宽度作为ul移动距离
        - 此时需要小圆圈的索引号，我们可以在生成小圆圈的时候，给它设置一个自定义属性，点击的时候获取这个自定义属性即可

      - 鼠标不经过轮播图，轮播图也会自动播放图片

        - 添加一个定时器
        - 自动播放实际类似于点击了右侧按钮
        - 此时使用**手动调用**右侧按钮**点击事件** arrow_r.click()

      - 鼠标经过，轮播图模块自动播放停止

  - 节流阀

    - 防止轮播图按钮连续点击造成播放过快

    - 节流阀目的：当上一个函数动画内容执行完毕，再去执行下一个函数动画，让事件无法连续触发

    - 核心实现思路：利用回调函数，添加一个变量来控制，锁住函数和解锁函数

      - 开始设置一个变量var flag = true；          

      - ```js
        if(flag)
            {
                flag = false;
                do something;
            }                      //关闭水龙头
        ```

      - 利用回调函数，动画执行完毕，flag = true 打开水龙头

  - 滚动窗口至文档的特定位置

    - window.scroll(x,y)      (里面的x和y不跟单位)
