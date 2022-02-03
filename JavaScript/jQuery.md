# jQuery

JavaScript库

- 仓库：可以把很多东西放到这个仓库里面。找东西只需要到仓库里面查找到就可以了。
- JavaScript库：即library，是一个**封装**好的特定**的集合**（方法和函数）。从封装的一大堆函数的角度理解库，就是在这个库中，封装了很多预先定义好的函数在里面，比如动画animate、hide、show，比如获取元素等。
- 简单理解：就是一个JS文件，里面对我们原生js代码进行了封装，存放到里面。这样我们可以快速高效地使用这些封装好的功能了。比如jQuery，就是为了快速方便地操作DOM，里面基本都是函数（方法）。

jQuery的概念

- **jQuery是**一个快速、简洁的**JavaScript库**，其设计的宗旨是“write Less,Do More"，即倡导写更少的代码，做更多的事情
- j就是JavaScript；Query查询；意思就是查询js，把js中的DOM操作做了封装，我们可以快速的使用查询里面的功能。
- **jQuery封装了JavaScript常用的功能代码**，优化了DOM操作、事件处理、动画设计和Ajax交互。
- **学习jQuery本质：就是学习调用这些函数（方法）**
- 优点：
  - 轻量级。核心文件才几十kb，不会影响页面加载速度
  - 跨浏览器兼容。基本兼容了现在主流的浏览器
  - 链式编程、隐式迭代
  - 对事件、样式、动画支持，大大简化了DOM操作
  - 支持插件扩展开发。有着丰富的第三方的插件，例如：树形菜单、日期控件、轮播图等
  - 免费、开源

jQuery的基本使用

- jQuery的下载

  - 官网地址：https://jquery.com/
  - 版本：
    - 1x：兼容IE678等低版本浏览器，官网不再更新
    - 2x：不兼容IE678等低版本浏览器，官网不再更新
    - 3x：不兼容IE678等低版本浏览器，是官方主要更新维护的版本
    - 各个版本的下载：https://code.jquery.com/

- jQuery的使用步骤

  - 引入jQuery文件
  - 使用即可

- jQuery的入口函数

  - ```js
    $(function () {
      ...         //此处是页面DOM加载完成的入口
    });
    ```

  - ```js
    $(document).ready(function () {
        ...          //此处是页面DOM加载完成的入口
    });
    ```

  -  等着DOM结构渲染完毕即可执行内部代码，不必等到所有外部资源加载完成，jQuery帮我们完成了封装

  - 相当于原生js中的DOMContentLoaded

  - 不同于原生js中的load事件是等页面文档、外部的js文件、css文件、图片加载完毕才执行内部代码

  - 更推荐使用第一种方式

- jQuery的顶级对象$

  - $$$是jQuery的别称,在代码中可以使用jQuery代替$$$,但一般为了方便，通常都直接使用$$
  - $$$是jQuery的顶级对象，相当于原生JavaScript中的window。把元素利用 $$$包装成jQuery对象，就可以调用jQuery的方法

- jQuery对象和DOM对象

  - 用原生JS获取来的对象就是DOM对象

  - jQuery方法获取的元素就是jQuery对象（如$('div'))

  - jQuery对象本质是：利用$对DOM对象包装后产生的对象（伪数组形式存储）

  - jQuery对象只能使用jQuery方法，DOM对象则使用原生的JavaScript属性和方法

  - DOM对象和jQuery对象之间是可以相互转换的：因为元素js比jQuery更大，原生的一些属性和方法jQuery没有给我们封装，要想使用这些属性和方法需要把jQuery对象转换为DOM对象才能使用

    - DOM对象转换为jQuery对象：**$(DOM对象)**

    - jQuery对象转换为DOM对象（两种方式）

      ```js
      $('div')[index]         //index是索引号
      $('div').get(index)         //index是索引号
      ```

      

#### jQuery常用API

- jQuery选择器            

  - jQuery基础选择器

    - 原生JS获取元素方式很多，很杂，而且兼容性情况不一致，因此jQuery给我们做了封装，使获取元素统一标准

      ```js
      $("选择器")            //里面选择器直接写CSS选择器即可，但是要加引号
      ```

    - | 名称       | 用法            | 描述                     |
      | ---------- | --------------- | ------------------------ |
      | ID选择器   | $('#id')        | 获取指定ID的元素         |
      | 全选选择器 | $('*')          | 匹配所有元素             |
      | 类选择器   | $(".class")     | 获取同一类class的元素    |
      | 标签选择器 | $("div")        | 获取同一类标签的所有元素 |
      | 并集选择器 | $("div,p,li")   | 选取多个元素             |
      | 交集选择器 | $("li.current") | 交集元素                 |

  - jQuery层级选择器

    - | 名称       | 用法        | 描述                                                         |
      | ---------- | ----------- | ------------------------------------------------------------ |
      | 子代选择器 | $("ul>li"); | 使用>号，获取亲儿子层级的元素；注意，并不会获取孙子层级的元素 |
      | 后代选择器 | $("ul li"); | 使用空格，代表后代选择器，获取ul下的所有li元素，包括孙子等   |

  - jQuery设置样式

    - ```js
      $('div').css('属性','值')
      ```

  - **<u>隐式迭代（重要）</u>**

    - 遍历内部DOM元素（伪数组形式存储）的过程就叫做**隐式迭代**
    - 简单理解：给匹配到的所有元素进行循环遍历，执行相应的方法，而不用我们再进行循环，简化我们的操作，方便我们调用

  - jQuery筛选选择器

    - | 语法       | 用法          | 描述                                                      |
      | ---------- | ------------- | --------------------------------------------------------- |
      | :first     | $('li:first') | 获取第一个li元素                                          |
      | :last      | $('li:last')  | 获取最后一个li元素                                        |
      | :eq(index) | $("li:eq(2)") | 获取到的li元素中，选择索引号为2的元素，索引号index从0开始 |
      | :odd       | $("li.odd")   | 获取到的li元素中，选择索引号为奇数的元素                  |
      | :even      | $('li:even')  | 获取到的li元素中，选择索引号为偶数的元素                  |

  - **<u>jQuery筛选方法（重点）</u>**

    - 

    | 语法                   | 用法                           | 说明                                                   |
    | ---------------------- | ------------------------------ | ------------------------------------------------------ |
    | **parent()**           | $("li").parent();              | 查找父级                                               |
    | **children(selector)** | $("ul").children("li");        | 相当于$("ul>li")，最近一级（亲儿子）                   |
    | **find(selector)**     | $("ul").find("li");            | 相当于$("ul li")，后代选择器                           |
    | **siblings(selector)** | $(".first").siblings("li");    | 查找兄弟节点，**不包括自己本身**                       |
    | nextAll([expr])        | $(".first").nextAll()          | 查找当前元素之后所有的同辈元素                         |
    | prevAll([expr])        | $(".last").prevtAll()          | 查找当前元素之前所有的同辈元素                         |
    | hasClass(class)        | $('div').hasClass("protected") | 检查当前的元素是否含有某个特定的类，如果有，则返回true |
    | **eq(index)**          | $("li").eq(2);                 | 相当于$("li:eq(2)"),index从0开始                       |

  - jQuery里面的排他思想

    - 想要多选一的效果，排他思想：当前元素设置样式，其余的兄弟元素清除样式

    - ```js
      $(function() {
                  $("button").click(function() {
                      $(this).css("background","pink");
                      //排除其他兄弟样式
                      $(this).siblings("button").css("background","");
                  })
              })
      ```

    - jQuery得到当前元素索引号**$(this).index()**

  - jQuery链式编程

    - 链式编程是为了节省代码量，看起来更优雅

      ```js
      $(this).css("color","red").sibling().css('color','');
      ```

    - 使用链式编程一定注意是哪个对象执行样式



- jQuery样式操作

  - 操作css方法

    - jQuery可以使用css方法来修改简单元素样式；也可以操作类，修改多个样式

    - 参数只写属性名，则是返回属性值

      ```js
      $(this).css("color");
      ```

    - 参数是**属性名，属性值，逗号分隔**，是设置一组样式，属性必须加引号，值如果是数字可以不用跟单位和引号

      ```js
      $(this).css("color","red");
      ```

    - 参数可以是对象形式，方便设置多组样式。属性名和属性值用冒号隔开，属性可以不用加引号

      ```js
      $(this).css(["color":"white","font-size","20px"]);
      ```

  - 设置类样式方法

    - 作用等同于以前的classList，可以操作类样式，注意操作类里面的参数不要加点

    - 添加类

      ```js
      $("div").addClass("current");
      ```

    - 移除类

      ```js
      $("div").removeClass("current");
      ```

    - 切换类

      ```js
      $("div").toggleClass("current");
      ```

  -  类操作与className的区别

    - 原生JS中className会覆盖元素原先里面的类名
    - jQuery里面类操作只是对指定类进行操作，不影响原先的类名

    

- jQuery效果

  - jQuery给我们封装了很多动画效果，最为常见的如下：

    - 显示隐藏：show（）、hide（）、toggle（）
    - 滑动：slideDown（）、slideUp（）、slideToggle（）
    - 淡入淡出：fadeIn（）、fadeOut（）、fadeToggle（）、fadeTo（）
    - 自定义动画：animate（）

  - 显示隐藏效果

    - 显示语法规范

      - ```js
        show([speed],[easing],[fn]])
        ```

      - 显示参数

        - 参数都可以省略，无动画直接显示
        - speed：三种预定速度之一的字符串（“slow”，“normal”，or “fast”）或表示动画时长的毫秒数值（如：1000）
        - easing：（Optional）用来指定切换效果，默认是“swing”，可用参数“linear”
        - fn：回调函数，在动画完成时执行的函数，每个元素执行一次

    - 隐藏语法规范

      - ```js
        hide([speed],[easing],[fn]])
        ```

      -  隐藏参数（同显示参数）

    - 切换语法规范

      - ```js
        toggle([speed],[easing],[fn]])
        ```

      - 切换参数（同显示参数）

  - 滑动效果

    - 下滑语法规范

      - ```js
        slideDown([speed],[easing],[fn]])
        ```

      - 下滑参数（同显示参数）

    - 上滑语法规范

      - ```js
        slideUp([speed],[easing],[fn]])
        ```

      - 上滑参数（同显示参数）

    - 切换滑动语法规范

      - ```js
        slideToggle([speed],[easing],[fn]])
        ```

      - 切换滑动参数（同显示参数）

  - 事件切换

    - ```js
      hover([over,]out)
      ```

    - over:鼠标移到元素上要触发的函数（相当于mouseenter）

    - out：鼠标移出元素要触发的函数（相当于mouseleave）

    - 如果只写一个函数，那么鼠标经过和离开都会触发这个函数

  - 动画队列及其停止排队方法

    - 动画或效果队列

      - 动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行

    - 停止排队

      - ```js
        stop()
        ```

      - stop()方法用于停止动画或效果

      - 注意：stop()写到动画或者效果的**前面，相当于停止结束上一次的动画**

  - 淡入淡出效果

    - 淡入效果语法规范

      - ```js
        fadeIn([speed],[easing],[fn]])
        ```

      - 淡入参数（同显示参数）

    - 淡出效果语法规范

      - ```js
        fadeOut([speed],[easing],[fn]])
        ```

      - 淡出参数（同显示参数）

    - 淡入淡出切换效果语法规范

      - ```js
        fadeToggle([speed],[easing],[fn]])
        ```

      - 淡入淡出切换参数（同显示参数）

    - 渐进方式调正到指定的不透明度

      - ```js
        fadeTo([speed],opacity,[easing],[fn]])
        ```

      - 效果参数：

        - **opacity透明度必须写，取值0-1之间**
        - **speed（必须写）**：三种预定速度之一的字符串（“slow”，“normal”，or “fast”）或表示动画时长的毫秒数值（如：1000）
        - easing：（Optional）用来指定切换效果，默认是“swing”，可用参数“linear”
        - fn：回调函数，在动画完成时执行的函数，每个元素执行一次

  - 自定义动画animate

    - 语法：

      ```js
      animate(params,[speed],[easing],[fn])
      
      animate({scrollTop:0},1000,linear,function() {})
      ```

    - 参数：

      - **params：想要更改的样式属性，以对象形式传递，必须写。属性名可以不用带引号，如果是复合属性则需要采取驼峰命名法borderLeft。**其余参数都可以省略。
      - speed：三种预定速度之一的字符串（“slow”，“normal”，or “fast”）或表示动画时长的毫秒数值（如：1000）
      - easing：（Optional）用来指定切换效果，默认是“swing”，可用参数“linear”
      - fn：回调函数，在动画完成时执行的函数，每个元素执行一次



- jQuery属性操作

  - 设置或获取元素固有属性值prop()

    - 所谓元素固有属性就是元素本身自带的属性，比如<a>元素里面的href，比如<input>元素里面的type

    - 获取属性语法

      ```js
      prop("属性")
      ```

    - 设置属性语法

      ```js
      prop("属性","属性值")
      ```

  - 设置或获取元素自定义属性值attr()

    - 用户自己给元素添加的属性，我们称为自定义属性。比如给div添加index = ”1“

    - 获取属性语法

      ```js
      attr("属性")           //类似原生getAttribute()
      ```

    - 设置属性语法

      ```js
      attr("属性","属性值")       //类似原生setAttribute()
      ```

    - 该方法也可以获取H5自定义属性

  - 数据缓存data（）

    - data（）方法可以在指定的元素上存取数据，并不会修改DOM元素结构。一旦页面刷新，之前存放的数据都将被移除

    - 附加数据语法

      ```js
      data("name","value")     //向被选元素附加数据
      ```

    - 获取数据语法

      ```js
      data("name")        //向被选元素获取数据
      ```

    - 同时还可以读取HTML5自定义属性data-index，得到的是数字型

      

- jQuery内容文本值

  - 主要针对**元素的内容**还有**表单的值**操作

  - 普通元素内容html()    （相当于原生inner HTML)

    ```js
    html()        //获取元素的内容
    html("内容")     //设置元素的内容
    ```

  - 普通元素文本内容text()        （相当于原生innerText）

    ``` js
    text()       //获取元素的文本内容
    text("文本内容")    //设置元素的文本内容
    ```

  - 表单的值val()      （相当于原生value）

    

- jQuery元素操作

  - 主要是**遍历**、创建、添加、删除元素操作

  - 遍历元素

    - jQuery隐式迭代是对同一类元素做了同样的操作。如果想要给同一类元素做不同操作，就需要用到遍历。

    - 语法1：

      - ```js
        $("div").each(function(index,domEle) { xxx; })
        
        $("div").each(function(i,domEle) {
                        //回调函数第一个参数一定是索引号，可以自己指定索引号名称
                        $(domEle).css("color",arr[i]);
                    })
        ```

      - each（）方法遍历匹配的每一个元素。主要用DOM处理。each每一个

      - 里面的回调函数有2个参数：index是每个元素的索引号；domEle是每个**DOM元素对象，不是jQuery对象**

      - **所以想要使用jQuery方法，需要给这个DOM元素转换为jQuery对象$(domEle)**

    - 语法2：

      - ```js
        $.each(object,function(index,element) { xxx; })
        
        arr = {
            name: "andy",
            age: 18
        }
        $.each(arr,function(i,ele) {
                        console.log(i);       //输出的是name age 属性名
                        console.log(ele);      //输出的是 andy 18 属性值
                    })
        ```

      - $.each()方法可用于遍历任何对象。**主要用于数据处理**，比如数组，对象
      - 里面的函数有2个参数：index是每个参数的索引号；element遍历元素

  - 创建元素

    - 语法：

      ```js
      $("<li></li>");
      ```

    - 动态的创建了一个<li>

  - 添加元素

    - 内部添加

      - ```js
        element.append("内容")      //把内容放入匹配元素内部最后面，类似原生appendChild
        ```

      - ```js
        element.prepend("内容")      //把内容放入匹配元素内部的最前面
        ```

    - 外部添加

      - ```js
        element.after("内容")         //把内容放入目标元素后面
        ```

      - ```js
        element.before("内容")        //把内容放入目标元素前面
        ```

  - 删除元素

    - ```js
      element.remove()       //删除匹配的元素（本身）
      ```

    - ```js
      element.empty()           //删除匹配的元素集合中所有的子节点
      ```

    - ```js
      element.html("")              //清空匹配的元素内容
      ```

      

- jQuery尺寸、位置操作

  - jQuery尺寸

    - | 语法                               | 用法                                                  |
      | ---------------------------------- | ----------------------------------------------------- |
      | width()/height()                   | 取得匹配元素宽度和高度值，只算width/height            |
      | innerWidth()/innerHeight()         | 取得匹配元素宽度和高度值，包含padding                 |
      | outerWidth()/outerHeight()         | 取得匹配元素宽度和高度值，包含padding、border         |
      | outerWidth(true)/outerHeight(true) | 取得匹配元素宽度和高度值，包含padding、border、margin |

    - 以上参数为空，则是获取相应值，返回的是数字型

    - 如果参数为数字，则是修改相应值

    - 参数可以不必写单位

  - jQuery位置

    - 位置主要有三个：offset（）、position（）、scrollTop（）/scrollLeft（）
    - offset（）设置或获取元素偏移
      - offset（）方法设置或返回被选元素**相对于文档**的偏移坐标，跟父级没有关系
      - 该方法有2个属性left、top。offset().top用于获取距离文档顶部的距离，offset().left用于获取距离文档左侧的距离
      - 可以设置元素的偏移：offset（{top：10，left：30}）；
    - position（）获取元素偏移
      - position（）方法用于返回被选元素**相对于带有定位的父级**偏移坐标，如果父级都没有定位，则以文档为准
      - 这个方法只能获取不能设置偏移
    - scrollTop（）/scrollLeft（）设置或获取元素被卷去的头部和左侧
      - scrollTop（）方法设置或返回被选元素被卷去的头部







#### jQuery事件

- jQuery事件注册

  - 单个事件注册

    - 语法：

      ```js
      element.事件(function(){})
      $("div").click(function(){事件处理程序})
      ```

    - 其他事件与原生基本一致

    - 比如mouseover、mouseout、blur、focus、change、keydown、keyup、resize、scroll等

  - 事件处理on（）绑定事件

    - on（）方法在匹配元素上绑定一个或多个事件的事件处理函数

    - 语法：

      ```js
      element.on(events,[selector],fn)
      
      如： $("div").on({
                      mouseenter: function() {
                          $(this).css("background","skyblue");
                      },
                      click: function() {
                          $(this).css("background","purple");
                      },
                      mouseleave: function() {
                          $(this).css("background","blue");
                      }
                  })
      ```

    - events：一个或多个用逗号分隔的事件类型

    - selector：元素的子元素选择器

    - fn：回调函数，即绑定在元素身上的侦听函数

      

    - on（）方法优势2：

      - 可以事件委派操作，事件委派的定义就是，把原来加给子元素身上的事件绑定在父元素身上，就是把事件委派给父元素

      - 如：

        ```js
        $("ul").on("click","li",function() {
            alert("hello world");
        })
        //click是绑定在ul上的，但是触发的对象是ul里面的小li
        ```

    - on（）方法优势3：

      - on可以给未来动态创建的元素绑定事件

      - 如：

        ```js
        $("ol").on("click","li",function () {
            alert(11);
        })
        var li = $("<li>我是后来创建的</li>");
        $("ol").append(li);
        ```

  - 事件处理off（）解绑事件

    - off（）方法可以移除通过on（）方法添加的事件处理程序

    - ```js
      $("p").off()      //解绑p元素所有的事件处理程序
      $("p").off("click")      //解绑p怨怒是上面的点击事件，后面的foo是侦听函数名
      $("ul").off("click","li")       //解绑事件委托
      ```

    - 如果有的事件只想触发一次，可以用one（）来绑定事件

      ```js
      $("p").one("click",function(){})
      ```

  - 自动触发事件trigger（）

    - 有些事件希望自动触发，比如轮播图自动播放功能跟点击右侧按钮一致，可以利用定时器自动触发右侧按钮点击事件，不必鼠标点击触发

    - ```js
      element.click()    //第一种简写形式
      element.trigger("type")   //第二种自动触发模式
      
      $("p").on("click",function() {
          alert("hi");
      });
      $("p").trigger("click");       //此时自动触发点击事件，不需要鼠标点击
      
      
      
      element.triggerHandler(type)         //第三种自动触发模式（不会触发元素的默认行为）   
      ```

      

- jQuery事件对象

  - 事件被触发，就有事件对象的产生

    - ```js
      element.on(events,[selector],function(event) {})
      ```

    - 阻止默认行为：event.preventDefault() 或者 return false

    - 阻止冒泡：event.stopPropagation()





#### jQuery其他方法

- jQuery对象拷贝

  - 如果想要把某个对象拷贝（合并）给另外一个对象使用，此时可以使用$.extend()方法

  - 语法：

    - ```js
      $.extend([deep],target,object1,[objectN])
      ```

    - deep：如果设为true为深拷贝，默认为false（浅拷贝）

    - target：要拷贝的目标对象

    - object1：待拷贝到第一个对象的对象

    - objectN：待拷贝到第N个对象的对象

    - 浅拷贝是把被拷贝对象**复杂数据类型中的地址**拷贝给目标对象，修改目标对象**会影响**被拷贝对象

    - 深拷贝，前面加true，完全克隆（拷贝的对象，而不是地址），修改目标对象**不会影响**被拷贝对象

    - 会覆盖目标对象中原有的数据

      

- jQuery多库共存

  - 问题概述：jQuery使用$$$作为标识符，随着jQuery的流行，其他js库也会用这$$$作为标识符，这样一起使用会引起冲突

  - 客观需求：需要一个解决方案，让jQuery和其他的js库不存在冲突，可以同时存在，这就叫做多库共存

  - jQuery解决方案：

    - 把里面的$符号统一改为jQuery，比如jQuery（“div”）

    - jQuery遍历规定新的名称：$.noConflict()       

      如var xx = $.noConflict();                         （此时xx等价于jQuery）

    

- jQuery插件

  - jQuery功能比较有限，想要更复杂的特效效果，可以借助于jQuery插件完成
  - 注意：这些插件也是依赖于jQuery来完成的，所以需要先引入jQuery文件，因此也成为jQuery插件
  - jQuery插件常用的网站：
    - jQuery插件库：http://www,jq22.com/
    - jQuery之家：http://www.htmleaf.com/
  - jQuery插件使用步骤：
    - 引入相关文件。（jQuery文件和插件文件）
    - 复制相关的html、css、js（调用插件）
  - jQuery插件演示
    - 瀑布流
    - 图片懒加载（图片使用延迟加载可提高网页下载速度。它也能帮助减轻服务器负载）
      - 即当我们页面滑动到可视区域，再显示图片
      - 我们使用jQuery插件库EasyLazyload。**注意：此时的js引入文件和js调用必须写到DOM元素（图片）的最后面**
    - 全屏滚动（fullpage.js)
      - gitHub:   https://github.com/alvarotrigo/fullPage.js
      - 中文翻译网站： http://www.dowebok.com/demo/2014/77/
  - bootstrap JS插件：
    - bootstrap框架也是依赖于jQuery开发的，因此里面的js插件使用，也必须引入jQuery文件





#### 数据可视化项目

- 什么是数据可视化
  - 数据可视化主要目的：借助于图形化手段，清晰有效得传达与沟通信息
  - 数据可视化可以把数据从冰冷的数字转换成图形，解释蕴含在数据中的规律和道理
- 数据可视化的场景
  - 通用报表
  - 移动端图标
  - 大屏可视化
  - 图编辑&图分析
  - 地理可视化
- 常见的数据可视化库
  - D3.js   目前Web端评价最高的JavaScript可视化工具库（入手难）
  - ECharts.js   百度出品的一个开源JavaScript数据可视化库
  - Highcharts.js  国外的前端数据可视化库，非商用免费，被许多外国大公司所使用
  - AntV蚂蚁金服全新一代数据可视化解决方案

- ECharts简介

  - ECharts是一个使用JavaScript实现的开源可视化库，可以流畅的运行在PC和移动设备上，兼容当前绝大部分浏览器（IE8/9/10/11，Chrome，firefox，Safari等），底层依赖矢量图形库ZRender，提供直观，交互丰富，可高度个性化定制的数据可视化图表。

- ECharts的基本使用

  - ECharts使用五部曲

    - 下载并引入echarts.js文件（图表依赖这个**js库**）

    - 准备一个具备大小的DOM容器（生成的图表会放入这个**容器**内）

    - 初始化echarts实例对象（**实例化**echarts对象）

    - 指定配置项和数据（option）             （根据具体需求修改**配置**选项）

    - 将配置选项设置给echarts实例对象（让echarts对象根据修改好的配置**生效**）

    - ```js
      <style>
              .box {
                  width: 400px;
                  height: 400px;
                  background-color: pink;
              }
          </style>
          <script src="./echarts.min.js"></script>
      </head>
      <body>
          <div class="box">
      
          </div>
      
          <script>
              var myChart = echarts.init(document.querySelector('.box'));
              var option = {
              title: {
                text: 'ECharts 入门示例'
              },
              tooltip: {},
              legend: {
                data: ['销量']
              },
              xAxis: {
                data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
              },
              yAxis: {},
              series: [
                {
                  name: '销量',
                  type: 'bar',
                  data: [5, 20, 36, 10, 10, 20]
                }
              ]
            };
            myChart.setOption(option);
          </script>
      ```

  - 相关配置讲解

    - title：标题组件

    - tooltip：提示框组件

    - legend：图例组件

    - toolbox：工具栏

    - grid：直角坐标系内绘图网络

    - xAxis：直角坐标系grid中的x轴

    - yAxis：直角坐标系grid中的y轴

    - series：系列列表。每个系列通过type决定自己的图表类型（什么类型的图标）

      - type：类型（什么类型的图表），比如line是折线，bar是柱形等

      - name：系列名称，用于tooltip的显示，legend的图例筛选变化

      - stack：数据堆叠。如果设置相同值，则会数据堆叠。

        数据堆叠：   第二个数据值 = 第一个数据值 + 第二个数据值

        ​                       第三个数据值 = 第二个数据值 + 第三个数据值......    依次叠加

          **如果给stack指定不同值或者去掉这个属性则不会发生数据堆叠**

    - color：调色盘颜色列表

  - 

  - 

  - 

  - 

  - 

  - 

  - d

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



















































d











































































































































