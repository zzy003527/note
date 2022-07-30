## Sass/less和tailwind css

#### less

- Less是一个**CSS预处理器**，Less文件后缀是**.less**

  - 好处：扩充了CSS语言，使CSS具备一定的逻辑性、计算能力
  - 注意：**浏览器不识别Less代码，目前阶段，网页要引入对应的CSS文件**

- 编译插件——Easy Less

  - vscode插件
  - 作用：less文件**自动生成**css文件

- Less语法

  - 单行注释

    语法：**//注释内容**                   快捷键：**ctrl+/**

  - 块注释

    语法：

    ```less
    /*注释内容*/           快捷键：shift+alt+A
    ```

  - 运算

    1.加 、减、乘直接书写计算表达式

    2.**除法**需要添加**小括号**或**.**

    ```less
    .box {
        width: 100+50px;
        height:5*32px;
        
        width: (100/4px);
        height:100./4px;
    }
    ```

  - Less嵌套

    1.作用：快速生成后代选择器

    2.语法：

    ```less
    /*
    .父级选择器 {                            
                                  //父级样式
                                           .子级选择器{
                                                                 //子级样式
                                                                }
                                          }
    */
    .father {
    color:red;
        .son 
        {
            width:100px;
        }
    }
    ```

    3.注意：&**不**生产后代选择器，表示**当前选择器**，通常配合**伪类或伪元素**使用

  - Less变量

    1.方法：把颜色提前存储到一个容器，设置属性值为这个容器名

    2.变量：存储数据，方便使用和修改

    3.语法：定义变量：**@变量名：值；**

    ​               使用变量：**CSS属性：@变量名；**

  - Less导入

    1.开发网页时，网页如何引入公共样式？

    CSS：书写link标签

    Less：导入

    2.导入：**@import”文件路径“；**

    ```less
    //如果是less文件，可以省略后缀.less
    @import’base.less';
        @import '01-体验less'；
    ```

  - Less导出

    1.配置插件：设置-->搜索EasyLess-->在setting.json中编辑-->添加代码（注意，必须是双引号）

    ```less
    添加   "less.compile": {
        "out":"../导出文件名/"
    }
    ```

    2.单文件导出（单独导出）

    ```less
    在要导出的文件最上方添加     //out:./导出文件夹名/文件名         （文件名可有可无）
    ```

    3.禁止导出：在less文件**第一行**添加： **//out：false**

- 






#### Sass

- css预处理器（Sass/Scss、Less、stylus）是什么

  - 概念：CSS预处理器用一种专门的变成语言，进行Web页面样式设计，然后再编译成正常的CSS文件，以供项目使用、CSS预处理器为CSS增加一些编程的特性，无需考虑浏览器的兼容性问题。

- Sass是什么

  - 网址：https://sass-lang.com/

    中文网址: https://www.sass.hk/

  - Sass是一种动态样式语言，由Ruby开发者设计和开发。Sass语法属于缩排语法，比css多出一些功能（如变量，嵌套，运算，混入（mixin），继承，指令，颜色处理，函数等），更容易阅读

  - Sass的工作方式是：在Sass源文件中写代码，然后由Sass程序（Sass编译器/转译器）将其转换为最终浏览器能认识的CSS文件

  - Sass和Scss的关系：Sass的缩排语法对习惯了css的开发者比较不容易上手。于是Sass语法进行了改良，成为了Scss，与原来的语法兼容，至少用{}取代了原来的缩进

- Sass配置

  - 文档：https://github.com/ritwickdey/vscode-live-sass-compiler

  - 步骤：

    - 下载插件Live Sass Compiler

    - 点击Live Sass Compiler【卸载】旁边的设置键

    - 点击扩展设置

    - 点击在setting.json中编辑

    - 然后加上这一段：

      ```js
       // 配置sass
          "liveSassCompile.settings.formats": [
              {
                  /*
                      :nested - 嵌套格式
                      :expanded - 展开格式
                      :compact - 紧凑格式
                      :compressed - 压缩格式
                  */
                  "format": "expanded",       //可定制的出口css样式（expanded,compact,compressed,nested）
                  "extensionName": ".css",
                  "savePath": "~/../css",              //为null :表示当前目录
                  "savePathReplacementPairs": null
              }
          ],
          // 排除目录
          "liveSassCompile.settings.excludeList": [
              "/**/node_modules/**",
              "/.vscode/**"
          ],
          //是否生成对应的map
          "liveSassCompile.settings.generateMap": true,
          // 是否添加兼容前缀，例如：-webkit-  -moz- 等
          "liveSassCompile.settings.autoprefix": [
              "> 1%",
              "last 2 versions"
          ] ,
          "explorer.confirmDelete": false
      ```

    

- Sass语法嵌套拓展与注释

  - 选择器嵌套

    - 如css写为：

      ```css
      .header {
          color: red
      }
      .header .box {
          color: green
      }
      ```

    - 改写成Sass写为：

      ```scss
      .header {
          color: red
              .box {
                  color: green
                  }
      }
      ```

    - 避免了重复输入父选择器，复杂的CSS结构更易于管理

  - 父选择器`&`

    - 在嵌套CSS规则时，有时也需要直接使用嵌套外层的父选择器，例如：当给某个元素设定hover样式时，或者当body元素有某个classname时，可以用**`&`**代表嵌套规则外层的父选择器

    - 例如有：

      ```css
      .container { width: 1200px; margin: 0 auto }
      .container a { color: #333 }
      .container a:hover { text-decoration: underline; color: #F00 }
      .container .top { border:1px #f2f2f2 solid; }
      .container .top-left { float: left;width: 200px }
      ```

    - 用sass写为：

      ```scss
      .container {
          width: 1200px;
          margin: 0 auto;
          a {
              color: #333,
                  &:hover {
                      text-decoration: underline; 
                      color: #F00    
                  }
          }
          .top {
              border:1px #f2f2f2 solid;
              &-left {
                  float: left;
                  width: 200px
              }
          }
      }
      ```

  - 属性嵌套

    - 有些CSS属性遵循相同的命名空间（namespace），比如font-family,font-size,font-weight都以font作为属性的命名空间。为了便于管理这样的属性，同时也为了避免了重复输入，Sass允许将属性嵌套在命名空间中

    - 例如：

      ```css
      .container a {
          color: #333;
          font-size: 14px;
          font-family: sans-serif;
          font-weight: bold;
      }
      ```

    - 用Sass写为：

      ```scss
      .container {
          a {
              color: #333;
              font: {
                  size:14px;
                  family: sans-serif;
                  weight: bold;
              }
          }
      }
      ```

    - **注意font:后面要加一个空格**

  - 占位符选择器%foo 必须通过 @extend

    - 有时，需要定义一套样式并不是给某个元素用，而是值通过@extend指令使用，尤其时在制作Sass样式库的时候，希望Sass能够忽略用不到的样式

    - 例如：

      ```scss
      .button%base {
          display: inline-block;
          margin-bottom: 0;
      }
      
      .btn-default {
          @extend %base
          color: #333;
          border-color: #ccc;
      }
      
      .btn-success {
          @extend %base;
          color: green;
      }
      ```

      

- Sass注释

  - `//`单行注释不会被编译到CSS文件中
  - `/* */`多行注释会被编译到CSS文件中

- Sass变量

  - Sass变量的定义

    - 定义规则

      1. 变量以美元符号**（$）开头**，后面根变量名
      2. 变量名时不易数组开头的可包含字母、数字、下划线、横线（连接符）
      3. 写法同CSS，即变量名和值之间用冒号（：）分隔
      4. 变量一定要**先定义，后使用**

    - 连接符与下划线

      - 通过连接符与下划线 定义的同名变量为同一变量，建议使用连接符：

        ```scss
        $font-size: 14px;
        $font_size: 16px;
        .container { font-size; $font-size }
        ```

  - 变量的作用域

    - 局部变量

      - 定义：在选择器内定义的变量，只能在选择器范围内使用

      - ```scss
        .container {
            $foint-size: 14px;
            font-size: $font-size;
        }
        ```

    - 全局变量

      - 定义后能全局使用的变量

      - 第一种：在选择器外面的最前面定义的变量

        ```scss
        $font-size: 16px;
        .container {
            font-size: $font-size;
        }
        .footer {
            font-size: $font-size;
        }
        ```

      - 第二种：使用`!global`标志定义全局变量

        - ```scss
          .container {
              $font-size: 16px !global;
              font-size: $font-size;
          }
          .footer {
              font-size: $font-size;
          }
          ```

  - 变量值类型

    - 变量值的类型可以有很多种，Sass支持6中主要的数据类型：

      -  数字：1，2，3，10px
      - 字符串：有引号字符串和无引号字符串，如：'foo'，'bar'，baz
      - 颜色：blue，#04a319，rgba(255,0,0,0.5)
      - 空值：null
      - 数组（list）：用空格或逗号做分隔符，1.5em,1em,0.2em,arial,sans-serif
      - maps：相当于JavaScript的object：（key1:value1,key2:value2)

    - 例如：

      ```scss
      $laryr-index: 10;
      $border-width: 13px;
      $font-base-family:'Open Sans',Helvetica,Sans-Serif;
      $top-bg-color:rgba(255,147,29,0.6);
      $block-base-padding:6px 10px 6px 10px;
      $bank-mode:true;
      $var:null;            //值null是其类型的唯一值。它表示缺少值，通常由函数返回以指示缺少后果
      $color-map: (color1: #fa0000,color2: #fbe200, color3: #95d7eb)
      $fonts: (serif: "Helvetica Neue",monospace: "Consolas")
      ```

    - 使用：

      - ```scss
        .container {
            @if $blank-mode {
                background-color: #000;
            }
            @else {
                background-color: #fff;
            }
            content: type-of($var);
            content: length($var);
            color: map-get($color-map,color2)
        }
        ```

      - **注意：list需要用`map-get`取值，如果取到的值为空，则转换为css的时候会将其忽略**

    - 默认值：

      ```scss
      $color: #333;
      //如果$color之前没定义就使用如下的默认值
      $color: #666 !default;
      .container {
          border-color: #color;
      }
      ```

      

- Sass导入`@import`

  - @import

    - Sass拓展了@import的功能，允许其导入Scss或Sass文件，被导入的文件将合并编译到同一个CSS文件中，另外，被导入的文件中所包含的变量或者混合指令（mixin）都可以在导入的文件中使用

    - 例如：

      ```scss
      //public.scss
      $font-base-color: #333
      ```

      在index.scss中使用

      ```scss
      @import 'public'
      $color: #666;
      .container {
          border-color: $color;
          color: $font-base-color
      }
      ```

    - 注意：跟我们普通css里面@import的区别

      - 普通css的@import后面要加`url(地址)`

    - 如下几种方式，都将作为普通的css语句，不会导入任何Sass文件

      1. 文件拓展名是css

      2. 文件名以http：//开头

      3. 文件名是 url(）

      4. @import包含media queries（媒体查询)

         ```scss
         @import 'public.css'
         @import url(public)
         @import "http://xxx.com/xxx"
         @import "landscape" screen and (orientation:landscape)
         ```

      

- Sass混合指令（mixin）

  - 混合指令（mixin）用于定义可重复使用的样式，混合指令可以包含所有的CSS规则，绝大部分Sass规则，甚至通过参数功能 引入变量，输出多样化的样式

  - 定义与使用混合指令`@mixin`

    - ```scss
      @mixin mixin-name() {
          /* CSS声明 */
      }
      ```

    - 例子：

      ```scss
      //定义
      @mixin block {
          width: 96%;
          margin-left: 2%;
          border-radius: 8px;
          border: 1px #f6f6f6 solid;
      }
      
      //使用
      .container {
          .block {
              @include block;
          }
      }
      ```

    - 还可以支持参数：

      ```scss
      //定义flex布局元素纵轴的排列方式
      @mixin flex-align($aitem) {
          -webkit-box-align: $aitem;
          -webkit-align-items: $aitem;
          -ms-flex-align: $aitem;
          align-items: $aitem;
      }
      
      .container {
          //@include flex-align(center)
          @include flex-align($aitem: center)
      }
      ```

    - 可变参数（参数个数未知）：

      ```scss
      @mixin linear-gradient($direction,$gradients...) {
          background-color: nth($gradients,1);        //1是指$gradients中的第一个
          background-image: linear-gradient($direction,$gradients);
      }
      
      .container {
          @include linear-gradient(to right,#F00,orange,yellow);
      }
      ```

    - **用`@include`引入mixin**

  - @mixin混入总结

    - mixin是可以重复使用的一组CSS声明

    - mixin有助于减少重复代码，只需声明一次，就可在文件中引用

    - 混合指令可以包含所有的CSS规则，绝大部分Sass规则，甚至通过参数功能引入变量，输出多样化的样式

    - 使用参数时建议加上默认值

      

- Sass`@extend`（继承）指令

  - 在设计网页的时候通常遇到这样的情况：一个元素使用的样式与另一个元素完全相同，但又添加了额外的样式。通常会在HTML中给元素定义两个class，一个通用样式，一个特殊样式

  - scss案例

    - | 标记    | 说明                 |
      | ------- | -------------------- |
      | info    | 信息！请注意这个信息 |
      | success | 成功！已经完成提交   |
      | warning | 警告！请不要提交     |
      | danger  | 错误！请进行更改     |

    - 所有警告框的基本样式：

      ```scss
      .alert {
          padding: 15px;
          margin-bottom: 20px;
          border: 1px solid transparent;
          border-radius: 4px;
          font-size: 12px;
      }
      ```

    - 不同警告框单独风格：

      ```scss
      .alert-info {
          @extend .alert;
          color: #31708f;
          background-color: #d9edf7;
          border-color: #bce8f1;
      }
      ....
      ```

  - 注意：

    - @extend在使用的时候可以多个继承，也可以多层继承

  - 占位符`%`

    - 被继承的css父类没有被实际应用，它的唯一目的就是扩展其他选择器

    - 对于该类，可能不希望被编译输出到最终的css文件中，它只会增加CSS文件的大小，不会被使用。

    - 这就是占位符选择器的作用

    - **占位符选择器类似于类选择器**，但是它们不是以句号（.）开头，而是**以百分号(%)开头**

    - 当在Sass文件中使用占位符选择器时，它可以用于扩展其他选择器，但不会被编译成最终的CSS

    - 改写：

      ```scss
      .alert {
          padding: 15px;
          margin-bottom: 20px;
          border: 1px solid transparent;
          border-radius: 4px;
          font-size: 12px;
      }
      
      .alert-info {
          @extend %alert;
          color: #31708f;
          background-color: #d9edf7;
          border-color: #bce8f1;
      }
      ....
      ```

      

- Sass运算（Operations）符的基本使用

  - 等号操作符

    - 所有数据类型都支持等号运算符

      | 符号 | 说明   |
      | ---- | ------ |
      | ==   | 等于   |
      | !=   | 不等于 |

    - 例1，数字比较：

      ```scss
      $theme:1;
      .container {
          @if $theme==1 {
              background-coor: red;
          }
          @else {
              background-color: blue;
          }
      }
      ```

    - 例2，字符串比较：

      ```scss
      $theme: "blue";
      .container {
          @if $theme != "blue" {
              background-coor: red;
          }
          @else {
              background-color: blue;
          }
      }
      ```

  - 关系（比较）运行符

    - | 符号              | 说明     |
      | ----------------- | -------- |
      | <        (lt)     | 小于     |
      | >        (gt)     | 大于     |
      | <=          (lte) | 小于等于 |
      | >=          (gte) | 大于等于 |

    - 例子：

      ```scss
      $theme: 3;
      .container {
          @if $theme >= 5 {
              background-coor: red;
          }
          @else {
              background-color: blue;
          }
      }
      ```

  - 逻辑运行符

    - | 符号 | 说明   |
      | ---- | ------ |
      | and  | 逻辑与 |
      | or   | 逻辑或 |
      | not  | 逻辑非 |

    - 例子：

      ```scss
      $width: 100;
      $height: 200;
      $last: false;
      div {
          @if $width > 50 and $height < 300 {
              font-size: 16px;
          }
          @else {
              font-size: 14px;
          }
          @if not $last {
              border-color: red;
          }
          @else {
              border-color: blue;
          }
      }
      ```

  - 数字操作符

    - | 符号 | 说明 |
      | ---- | ---- |
      | +    | 加   |
      | -    | 减   |
      | *    | 乘   |
      | /    | 除   |
      | %    | 取模 |

    - 数字类型包括：纯数字、百分号、css部分单位（px、pt、in...）

    - 注意：

      - %和单位不能一起运算
      - 纯数字与**百分号或单位**运算   时会自动转换成相应的百分号与单位值
      - 乘法和除法**不能同时有多个单位或者百分号存在**
      - 遇到不同单位时会**换算统一单位后合并**

    - 以下三种情况，`/``将被视为除法运算符号：

      - 如果值或值的一部分，是变量或者函数的返回值
      - 如果值被圆括号包裹
      - 如果值是算数表达式的一部分

    - 例子：

      ```scss
      $width: 1000px;
      div　{
          font: 16px/30px Arial,Helvetica,sans-serif;          //不运算
          width: ($width/2);               //使用了变量与括号
          z-index: round(10)/2;          //使用了函数
          height: (500px/2);             //使用了括号
          margin-left: 5px + 8px/2px;          //使用了+表达式
      }
      ```

    - 如果需要使用变量，同时又要确保`/`不做触发运算而是完整地编译到CSS文件中，只需要使用`#{}`插值语句将变量包裹

  - 字符串运算

    - 可用于连接字符串

    - 注意：如果有引号字符串（位于+左侧）连接无引号字符串，运算结果是有引号的，相反，无引号字符串（位于+左侧）连接有引号字符串，运算结果则没有引号

    - 例子：

      ```scss
      .container {
          content: "Foo " + Bar;                 // "FooBar"
          font-family: sans- + "serif";          //sans-serif
      }
      ```

  - 插值语句`#{}`

    - 使用方法：`#{变量名}`

    - 例子：

      ```scss
      $width1: 12px;
      $width2:16px;
      $name: height;
      .container {
          width: #{$width1} / #{$width2};
          #{$name}: 18px;
      }
      
      
      //编译结果
      .container {
          width: 12px / 16px;
          height: 18px;
      }
      ```

      

- Sass常见函数的基本使用

  - 文档：https://sass-lang.com/documentation/modules

  - Color（颜色函数）

    - Sass包含很多操作颜色的函数。例如：lighten() 与 darken()函数可以用于调亮或调暗颜色，opacify()函数使颜色透明度减少，transparent()函数使颜色透明度增加，mix()函数课用来混合两种颜色

    - 例子：

      ```scss
      p {
          height: 30px;
      }
      
      .p0 {
          background-color: #5c7a29;
      }
      
      .p1 {
          /*
          让颜色变亮
          lighten($color,$amount)
          $amount 的取值在 0%-100%之间
          */
          background-color: lighten(#5c7a29,30%);
      }
      
      .p2 {
          //  让颜色变暗，通常使用color.scale()代替该方案
          background-color: darken(#5c7a29,15%);
      }
      
      .p3 {
          // 降低颜色透明度，通常使用color.scale()代替该方案
          // background-color: opacify(#5C7A29,0.5);
          background-color: opacify(rgba(#5c7a29,0.1),0.5);
      }
      ```

  - Math（数值函数）

    - 数值函数处理数值计算，例如：parcentage()将无单元的数值转化为百分比，round()将数字四舍五入为最接近的证书，min()和max()获取几个数字中的最小值或最大值，random()返回一个随机数

    - 例如：

      ```scss
      p {
          z-index: abs($number: -15);        //15
          z-index: ceil(5.8);      //6
          z-index: max(5,1,6,8,3);            //8
          opacity: random();            //随机0-1
      }
      ```

  - List函数

    - List函数操作List，length()返回列表长度，nth()返回列表中的特定项，join()将两个列表连接在一起，append()在列表末尾添加一个值

    - 例如：

      ```scss
      p {
          z-index: length(12px);           //1
          z-index: length(12px 5px 8px);         //3
          z-index: index(a b c d,c);            //3
          padding: append(10px 20px,30px);             //10px 20px 30px
          color: nth($list: red blue green,$n: 2);        //blue
      }
      ```

  - Map函数

    - Map函数操作Map，map-get()根据键值获取map中的对应值，map-merge()将两个map合并成一个新的map，map-values()映射中的所有值,map-has-key()判断map中是否有该值

    - 例如：

      ```scss
      $font-sizes: ("small": 12px,"normal": 18px,"large": 24px);
      $padding: (top:10px,right:20px,bottom:10px,left:30px);
      p {
          font-size: map-get($font-sizes,"normal");        //18px
          @if map-has-key($padding,"right") {
              padding-right: mao-get($padding,"right");
          }
          &:after {
              content: map-keys($font-sizes) +　"" + map-values($padding) +　""
                  //'"small","normal","large" 10px,20px,10px,30px'
          }
      }
      ```

  - Selector选择器函数

    - 选择符相关函数可对CSS选择进行一些相应的操作，例如：selector-append()可以把一个选择符附加到另一个选择符，selector-unify()将两组选择器合成一个复合选择器

    - 例如：

      ```scss
      .herder {
          background-color: #000; 
          //这里加''是为了让结果变为字符串
          content: selector-append(".a",".b",".c") + '';             //  ".a.b.c" 
          content: selector-unify("a",".disabled") + ''                // "a.disabled"
      }
      ```

  - 自检函数

    - 自检相关函数，例如：feature-exists()检查当前Sass版本是否存在某个特性，variable-exists()检查当前作用域中是否存在某个变量，mixin-exists()检查某个mixin是否存在

    - 例如：

      ```scss
      $color: #F00;
      @mixin padding($left:0,$top:0,$right:0,$bottom:0) {
          padding: $top $right $bottom $left;
      }
      
      .container {
          @if variable-exists($color) {
              color: $color;
          }
          @else {
              content: "$color不存在";
          }
          @if mixin-exists(padding) {
              @include padding($left: 10px,$right: 10px);
          }
      }
      ```

    - 自检函数通常用在代码的调试上-

      

    

- Sass流程控制指令

  - `@if`控制指令

    - @if()函数运行你根据条件进行分支，并仅返回两种可能结果中的一种

    - ```scss
      .container {
          @if() {
              
          }
          @else if() {
              
          }
          @else {
              
          }
      }
      ```

  - `@for`指令

    - @for指令可以在限制的范围内重复输出格式，每次按要求（变量的值）对输出结果做出被动。这个指令包含两种格式：

      `@for $var from <start> through <end>`或者`@for $var from <start> to <end>`

    - 区别就在于through与to的含义：

      - 当使用**through**时，条件范围**包含`<start>`和`<end>`的值**
      - 而使用**to**时，条件范围**只包含`<start>`的值**而不包含`<end>`的值
      - 另外，$$$var可以是任何变量，比如 $$$ i；**`<start>`和`<end>`必须是整数值**

    - 例子：

      ```scss
      @for $i from 1 to 4 {
          .p#{$i} {
              width: 10px * $i;
              height: 30px;
              background-color: red;
          }
      }
      
      @for $i from 1 through 3 {
          .p#{$i} {
              width: 10px * $i;
              height: 30px;
              background-color: red;
          }
      }
      ```

  - `@each`指令

    - @each指令的格式是` @each $var in <list>`,$var可以是任何变量名，比如`$length`或者`$name`，而`<name>`是一连串的值，也就是值列表。

    - 例子：

      ```scss
      $color-list: red green blue turquoise darkmagenta;
      @each $color in $color-list {
          $index: index($color-list,$color)
          .p#{$index - 1} {
              background-color: $color;
          }
      }
      ```

  - `@while` 指令

    - while指令重复输出格式知道表达式返回结果为false。这样可以实现比@for更复杂的循环

    - ```scss
      @while 条件 {
          //语句
      }
      ```

      

- Sass`@function`的使用

  - 函数作用

    - 把一些比较复杂或经常用的内容进行抽离（封装），以便重复使用

  - 函数的定义与使用

    - 函数的定义

      - ```scss
        @function function-name([$param1,$param2,...]) {
            ...
            @return $value;
        }
        ```

      - 注意：函数名function-name与function_name是相同的

    - @return

      - 它只允许在`@函数体`中使用，并且每个`@function`必须以`@return`介绍。当遇到@return时，它会立即结束函数并返回其结果

    - 函数的使用

      - 例如：

        ```scss
        @function row-cols-width($column) {
            @return percentage(1 / $column);
        }
        
        @for $i from 1 through 6 {
            .row-cols-#{$i} > * {
                width: row-cols-width($i);
            }
        }
        ```

      - 可以省略默认值和按照Key进行传参：

        ```scss
        @function background-linear-gradient($direction,$start-color,$end-color:blue) {
            @return linear-gradient($direction,$start-color,$end-color);
        }
        
        //正常传参
        body {
            background-image: background-linear-gradient(to right,blue,green);
        }
        
        //省略默认值
        body {
            background-image: background-linear-gradient(to right,blue);
        }
        
        //按照Key进行传参
        body {
            background-image: background-linear-gradient($start-color: red,$direction: to right);
        }
        ```

      - 也可以有任意参数

        ```scss
        /*
        定义线性渐变
        $direction 方向
        $gradients 颜色过渡的值列表
        */
        
        @function background-linear-gradient($direction,$gradients...) {
            @return linear-gradient($direction,$gradients);
        }
        .header {
            background-image: background-linear-gradient(to bottom,red,green,blue);
        }
        ```

      - 调用任意参数

        ```scss
        @width: 50px,30px,80px;
        .logo {
            width: min($width...);
        }
        ```

  - 混入mixin和函数funciton的区别

    - 混入mixin主要是通过传递参数的方式输出多样化的方式，为了可以实现代码复用
    - 函数的功能主要是通过传递参数后，经过函数内部的计算，最后@return输出一个值

    

- 三元条件函数if的使用

  - 如何使用：

    ```scss
    if($condition,$if-true,$if-false);
    ```

    判断$condition，如果条件成立，则返回`$if-true`的结果；如果条件不成立，则返回`$if-false`的结果

  - 例子：

    ```scss
    .container {
        color: if($theme == 'light'),#000,rgb(7,4,4));
    }
    ```

    

- Sass`@use`的使用

  - 作用：从其他Sass样式表加载mixin，function和变量，并将来自多个样式表的CSS组合在一起，@use加载的样式表被称为模块，**多次引入只包含一次**。@use也可以看作是对@import的增强

  - 语法：

    ```scss
    @use '<url>' [as alias | namespace]
    ```

  - 注意：

    - []中的内容为自定义名字

  - 加载普通Scss、css

    - use下面的_common.scss

      ```scss
      $font-size:14px !default;
      * {
          margin: 0;
          padding: 0;
          font-size: $font-size;
          color: #333;
      }
      
      @function column-width($col,$total) {
          @return percentage($col/$total);
      }
      
      @mixin bgColor($bg-color: #f2f2f2) {
          background-color: $bg-color;
      }
      ```

  - @use的方式

    ```scss
    @use 'use/common';
    @use 'use/global' as g1;
    @use 'use/global' as g2;
    body {
        font-size: common.$font-size;
        @include g1.base('#FFF');
        @include g2.base('#000');
        width: common.column-width(3,12);
        @include common.bgColor('#F00');
    }
    ```

    通过@use引入的样式**默认把文件名作为模块名使用**，你可以通过as的形式重新取一个别名

  - @use取消别名

    - 可能`@use '<url>' as *`来取消命名空间，这种方式加载的模块**被提升为全局模块**

    - 注意：这种方式慎用

      ```scss
      @use 'use/common';
      @use 'use/global' as *;
      @use 'use/global' as g2;
      body {
          font-size: common.$font-size;
          @include base('#FFF');
          @include g2.base('#000');
          width: common.column-width(3,12);
          @include common.bgColor('#F00');
      }
      ```

  - 默认创建index.scss

    - 创建_index.scss

      ```scss
      @use 'common' with ( $font-size:16px );
      @use 'global' as *;
      @use 'global' as g2;
      common.$font-size: 28px;         //也可能通过这种方式覆盖
      body {
          font-size: common.$font-size;
          @include base('#FFF');
          @include g2.base('#000');
      }
      ```

      

  - @use总结

    - @use引入同一个文件多次，不会重复引入，而@import会重复引入

    - @use引入的文件都是一个模块，默认以文件名作为模块名，可通过as alias取别名

    - @use引入多个文件时，每个文件都是单独的模块，相同变量名不会覆盖，通过模块名访问，而@import变量会被覆盖

    - @use方式可通过`@use 'xxx' as *`来取消命名空间，建议不要这么做

    - @use模块内可通过`$-`或者`$`来定义私有成员，也就是说以`-或者$`开头的Variables，mixins，functions不会被引入

    - @use模块内变量可通过!default定义默认值，引入时可通过`with(...)`的方式修改

    - 可定义-index.scss或_index.scss来合并多个scss文件，它@use默认加载文件

      

- Sass`@forward`的使用

  - 作用：通过`@forward`加载一个模块的成员，并将这些成员当作自己的成员对外暴露出去，类似于es6的export...，通常用于跨多个文件组织Sass库

  - 转发、合并Sass

    - 创建uses/_global.scss

      ```scss
      $font-size: 28px;
      @mixin base($color: #F00) {
          color: $color;
          font-size: $font-size;
      }
      
      .gclass {
          background-color: #F00;
      }
      ```

    - 创建uses/_common.scss

      ```scss
      $font-size:14px !default;
      * {
          margin: 0;
          padding: 0;
          font-size: $font-size;
          color: #333;
      }
      
      @function column-width($col,$total) {
          @return percentage($col/$total);
      }
      
      @mixin bgColor($bg-color: #f2f2f2) {
          background-color: $bg-color;
      }
      ```

    - 创建启动合并bootstrap.scss(这是一个单独的文件，专门用来利用forward来转发内容，相当于中间站)

      ```scss
      @forward 'uses/global'
      @forward 'uses/common'
      ```

    - 使用

      ```scss
      @use 'bootstrap';
      body {
          font-size: bootstrap.$font-size;
          width: bootstrap.column-width(3,12);
          @include bootstrap.base(#F00);
      }
      ```

      

  - 选择性转发

    - 通过`hide`或者`show`

    - ```scss
      @forward 'uses/common' hide bgColor;
      //这样子就隐藏了common.scss中的bgColor属性
      ```

    - ```scss
      @forward 'uses/common' show bgColor;
      //这样子就只转发了common.scss中的bgColor属性
      ```

  - 转发时定义前缀

    - 通过`as -*`实现

    - ```scss
      @forward 'uses/common' as c-* hide bgColor;
      //如此处调用common的column-width函数属性，要用：c-column-width
      ```

    - **加上前缀后每次调用该文件的属性都要加上前缀**

      

- Sass中`@at-root`的使用

  - 作用：`@at-root`可以使被嵌套的选择器或属性跳出嵌套

  - 语法：

    ```scss
    @at-root <selector> {
        ...
    }
    ```

  - 普通嵌套

    ```scss
    .parent {
        font-size: 12px;
        .child {
            font-size:14px;
            .son {
                font-size: 16px;
            }
        }
    }
    ```

  - 作用某个选择器使其跳出嵌套

    ```scss
    .parent {
        font-size: 12px;
        @at-root .child {
            font-size:14px;
            @at-root .son {
                font-size: 16px;
            }
        }
    }
    ```

  - 作用某些选择器使其跳出嵌套（用一个大括号把他们包起来）

    ```scss
    .parent {
        font-size: 12px;
        @at-root {
            .child-1 {
                font-size: 16px;
            }
            .child-2 {
                font-size: 16px;
            }
            .child-3 {
                font-size: 16px;
            }
        }
    }
    ```

  - @at-root(without:...)和@at-root(with:...)的使用

    - 默认@at-root只会跳出选择器嵌套，而不能跳出@media或@support，如果要跳出这两种，则需使用@at-root(without：media)，@at-root(without：support)。

    - 这个语法的关键词有四个：

      - all（表示所有）
      - rule（表示常规css）
      - media（表示media）
      - supports（表示supports）

    - 演示：

      ```scss
      @media screen {
          .parent {
              font-size: 12px;
              @at-root(without:media) {
                  .child {
                      font-size: 14px;
                      .son {
                          font-size: 16px;
                      }
                  }
              }
          }
      }
      ```

    - 









### tailwind css

- 什么是Tailwind CSS

  - 就是一个CSS框架，和bootstrap，element ui，Antd，bulma一样。将一些css样式封装好，用来加速我们开发的一个工具。

- 和其他的CSS框架有什么区别？

  - CSS发展到现在，基本经历了三个阶段。

  - 第一个阶段，原生写法

    - 是类似于编程中面向过程的写法，需要什么样式，自己在css中写什么样式。对代码有洁癖的程序员会进行简单的css复用。但是也只是简单的复用，大多数时候还是需要什么写什么，想怎么写怎么写。

  - 第二个阶段，CSS组件化

    - 类似于编程中面向对象的写法，将相同视觉的UI封装成一个组件。比如一个按钮，整个项目中，这个按钮被多次使用，并且样式一致。那么就可以封装成一个按钮类。使用的时候直接使用这个类名称就OK。

      这也是bootstrap，element ui，Antd，bulma的做法。

  - 第三个阶段，CSS零件化。

    - 也叫做CSS原子化。和上面第一个阶段第二个阶段都有类似的地方。依旧是组件，只是每个组件都是一个单一功能的css属性。

  - Tailwind CSS就是第三个阶段的产物，它做了什么呢？
    它将所有的css属性全部封装成语义化的类，比如你想要一个`float:left`，它已经帮你封装好了，你直接使用一个`float-left`就可以。

    需要一个宽度为12像素，只需要写`w-3`就可以。

  - 例子：

    - 你可能需要实现下面这样一个效果：![img](https://wyz.xyz/assets/files/2021-01-23/1611367178-123918-image.png)

    - 按照我们正常的写法，需要这样写

      ```html
      <div class="chat-notification">
        <div class="chat-notification-logo-wrapper">
          <img class="chat-notification-logo" src="/img/logo.svg" alt="ChitChat Logo">
        </div>
        <div class="chat-notification-content">
          <h4 class="chat-notification-title">ChitChat</h4>
          <p class="chat-notification-message">You have a new message!</p>
        </div>
      </div>
      
      <style>
        .chat-notification {
          display: flex;
          max-width: 24rem;
          margin: 0 auto;
          padding: 1.5rem;
          border-radius: 0.5rem;
          background-color: #fff;
          box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        .chat-notification-logo-wrapper {
          flex-shrink: 0;
        }
        .chat-notification-logo {
          height: 3rem;
          width: 3rem;
        }
        .chat-notification-content {
          margin-left: 1.5rem;
          padding-top: 0.25rem;
        }
        .chat-notification-title {
          color: #1a202c;
          font-size: 1.25rem;
          line-height: 1.25;
        }
        .chat-notification-message {
          color: #718096;
          font-size: 1rem;
          line-height: 1.5;
        }
      </style>
      ```

    - 但是使用Tailwind CSS，你只需要这样写就可以

      ```html
      <div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-md flex items-center space-x-4">
        <div class="flex-shrink-0">
          <img class="h-12 w-12" src="/img/logo.svg" alt="ChitChat Logo">
        </div>
        <div>
          <div class="text-xl font-medium text-black">ChitChat</div>
          <p class="text-gray-500">You have a new message!</p>
        </div>
      </div>
      ```

    - 极大的减少了代码量。

- Tailwind CSS有什么优点？

  - 可定制化程度极高

  - 不需要再写css。

  - 不需要再为class取个什么名字而苦恼。

  - 响应式设计

    - 比如你要实现一个媒体查询，根据不同的屏幕宽度实现不同的图片宽度。

    - 按照之前的写法，你可能得这么干

      ```css
      @media only screen and (max-width:1280px) { 
          .img {     
          width:196px; 
          } 
      }
      @media only screen and (max-width: 760px) { 
          .img {     
          width:128px; 
          } 
      }
      ```

    - 但是在Tailwind CSS，一句话就能搞定：

      ```html
      <img class="w-16 md:w-32 lg:w-48" src="...">
      ```

    

- tailwind css 2 安装使用

  - 安装PostCSS

    - 新建一个项目目录，然后在根目录运行`npm i --save-dev postcss-cli`

  - 安装Tailwind CSS以及相关依赖

    - `npm install tailwindcss@latest postcss@latest autoprefixer@latest`

  - 添加Tailwind作为PostCSS插件

    - 在根目录新建一个postcss配置文件`postcss.config.js`,并且将文件内容修改为以下：

      ```js
      // postcss.config.js
      module.exports = {
        plugins: {
          tailwindcss: {},
          autoprefixer: {},
        }
      }
      ```

  - 创建配置文件

    - 运行以下命令`npm init -y`，这会在根目录下生成一个`package.json`文件。

    - 继续运行：`npx tailwindcss init`，这会在你的项目根目录创建一个`tailwind.config.js`文件，将内容修改为以下：

      ```js
      // tailwind.config.js
      module.exports = {
        purge: [],
        darkMode: false, // or 'media' or 'class'
        theme: {
          extend: {},
        },
        variants: {},
        plugins: [],
      }
      ```

  - 创建css文件

    - 创建一个css文件，位置随意，比如`src/style.css`,并且将内容修改为如下：

      ```css
      /* ./你的css文件夹/styles.css */
      @tailwind base;
      @tailwind components;
      @tailwind utilities;
      ```

  - 创建构建命令

    - 现在打开 `package.json` 文件，添加以下运行脚本：

      ```js
      "scripts": {
        "build": "postcss src/style.css -o dist/tailwind.css"
      }
      ```

  - 创建Tailwind CSS

    - 这个构建的css文件就是你最终使用的css文件。
    - 现在，从命令行运行 `npm run build` 将构建最终的CSS文件。
    - 生成的文件位于 `dist/tailwind.css` （您可以在上面的命令中更改位置）。
    - 然后在你的html文件中引用这个css文件就可以。

  - 更改文件后自动重新生成CSS

    - 如果你想要每次修改文件后，自动重新生成一下css文件，比如你在`style.css`里自定义了一个组件，并且希望实时生效，那么就需要用到监听。

    - 现在打开 `package.json` 文件，添加以下运行脚本：

      ```js
      "scripts": {
        "watch": "postcss build src/css/tailwind.css -o dist/css/tailwind.css --watch",
      }
      ```

    - 现在 `package.json` 文件的脚本部分变成了这样：

      ```js
       "scripts": {
          "build": "tailwindcss build src/css/tailwind.css -o dist/css/tailwind.css",
          "watch": "postcss build src/css/tailwind.css -o dist/css/tailwind.css --watch",
        },
      ```

    - 这时候只要运行`npm run watch`就可以自动监听你的页面改动。

  - 优化Tailwind CSS文件大小

    - 上面编译出来的Tailwind是完整的css文件，总共18万行css代码，文件将近4M。但是我们实际的项目，其实根本用不着所有Tailwind CSS自带的样式，所以这时候，我们就需要优化一下，我们希望最终打包的时候，css只编译我们用到的部分，用不到的自动删除掉。

    - 这时候需要配置一下`tailwind.config.js`:

      ```js
       // tailwind.config.js
        module.exports = {
         purge: [
           './src/*.html',
         ],
          darkMode: false, // or 'media' or 'class'
          theme: {
            extend: {},
          },
          variants: {},
          plugins: [],
        }
      ```

    - 可以看到我们在 "purge "选项内增加了一个文件路径。意思就是打包的时候，检查这个路径下的这些文件，然后自动删除这些文件没用到的css。

    - 现在打开 `package.json` 文件，添加以下运行脚本：

      ```js
      "scripts": {
       "build:css": "cross-env NODE_ENV=production postcss src/css/tailwind.css -o dist/css/tailwind.css"
      }
      ```

    - 现在 `package.json` 文件的脚本部分变成了这样：

      ```js
        "scripts": {
          "build": "tailwindcss build src/css/tailwind.css -o dist/css/tailwind.css",
          "watch": "postcss build src/css/tailwind.css -o dist/css/tailwind.css --watch",
          "build:css": "cross-env NODE_ENV=production postcss src/css/tailwind.css -o dist/css/tailwind.css"
        },
      ```

    - 因为这条命令需要用到`cross-env`,所以安装一下：`npm i cross-env --save-dev`

    - 注意：

      - **在整个项目完成之后，运行`npm run build:css`,会自动构建一个最小的css文件。**

      - **在项目开发阶段，只需要运行`npm run watch`就可以。**

        

- Tailwind CSS 3 的安装使用

  - 安装Tailwind CSS

    - `tailwindcss` 通过 npm 安装，并创建 `tailwind.config.js` 文件。

    - ```bash
      npm install -D tailwindcss
      npx tailwindcss init
      ```

  - 配置模板路径

    - 在 `tailwind.config.js` 文件中添加所有模板文件的路径。

    - ```js
      module.exports = {
        content: ["./src/**/*.{html,js}"],
        theme: {
          extend: {},
        },
        plugins: [],
      }
      ```

  - 将Tailwind指令添加到CSS

    - 创建一个css文件，位置随意，比如`src/main.css`,并且将内容修改为如下：

      ```css
      @tailwind base;
      @tailwind components;
      @tailwind utilities;
      ```

  - 开始在HTML中使用Tailwind

    - 将已编译的 CSS 文件添加到 `<head>` 并开始使用 Tailwind 来设置内容样式。

    - ```html
      <!doctype html>
      <html>
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link href="/dist/output.css" rel="stylesheet">
      </head>
      <body>
        <h1 class="text-3xl font-bold underline">
          Hello world!
        </h1>
      </body>
      </html>
      ```

  - 创建构建命令

    - 运行以下命令`npm init -y`

    - 现在打开 `package.json` 文件，添加以下运行脚本：

      ```js
      "scripts": {
        "build": "tailwindcss -i ./src/main.css -o ./dist/output.css --watch"
      }
      ```

    - **这时候只要运行`npm run build`就可以自动监听你的页面改动并且实时编译了。**

    

- 使用cdn引入

  - ```html
    <link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet">
    ```

- 基础语法

  - 字符大小设置

    - 要设置字体大小，请使用 `text- {size}`。大小可以取 13 个值。相应的 CSS 样式在括号中。

    - ```css
      .text-xs（字体大小：.75rem；）
      .text-sm（字体大小：.875rem；）
      .text-base（字体大小：1rem;）
      .text-lg（字体大小：1.125rem；）
      .text-xl（字体大小：1.25rem；）
      .text-2xl（字体大小：1.5rem；）
      .text-3xl（字体大小：1.875rem；）
      .text-4xl（字体大小：2.25rem；）
      .text-5xl（字体大小：3rem；）
      .text-6xl（字体大小：4rem；）
      .text-7xl（字体大小：4.5rem；）
      .text-8xl（字体大小：6rem；）
      .text-9xl（字体大小：8rem；）
      ```

  - 字符粗细设置

    - 要设置字符粗细，请使用 `font- {thickness}`。厚度可以取 9 个值。相应的 CSS 样式在括号中。

    - ```css
      .font-thin (font-weight: 100;)
      .font-extralight (font-weight: 200;)
      .font-light (font-weight: 300;)
      .font-normal (font-weight: 400;)
      .font-medium (font-weight: 500;)
      .font-semibold (font-weight: 600;)
      .font-bold（font-weight：700；）
      .font-extrabold（font-weight：800；）
      .font-black（font-weight：900；）
      ```

  - 文字颜色设置

    - 要设置文本颜色，请使用 `text- {color}-{color depth}`。颜色可以设置为白色、黑色、红色、蓝色、靛蓝色……等。颜色强度可以取 **9** 个值。例如，在绿色的情况下，如下所示。

    - ```css
      text-green-100（颜色：#f0fff4;）
      text-green-200（颜色：#c6f6d5；）
      text-green-300（颜色：#9ae6b4；）
      text-green-400（颜色：#68d391；）
      text-green-500（颜色：#48bb78；）
      text-green-600（颜色：#38a169；）
      text-green-700（颜色：#2f855a；）
      text-green-800（颜色：#276749；）
      text-green-900（颜色：#22543d；）
      ```

    - 如果要将文本颜色更改为红色而不是绿色，可以像 text-red-500 一样更改它。如果要更改背景颜色，可以使用 bg-red-500 进行设置。

  - Tailwind CSS 自定义(components是用来自定义类的)

    - 打开预构建的 `css / style.css` 文件并在` @components` 和 `@utilities` 指令之间添加以下内容。

      ```css
      @tailwind base;
      
      @tailwind components;
      
      @layer components {
          .btn{
          @apply font-semibold text-white py-2 px-4 rounded;
          }
      }
      
      
      @tailwind utilities;
      ```

    - 然后重新构建一下，`npm run build`

    - 会覆盖构建完后的`public/css/style.css`，所以打开`style.css`文件，搜索`btn`

    - 可以看到刚才用`@apply `添加的内容已经作为css添加到`style.css`文件中了,

      ```css
      .btn{
        font-weight: 600;
        color: #fff;
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        padding-left: 1rem;
        padding-right: 1rem;
        border-radius: 0.25rem;
      }
      ```

  - 写css原生代码(utilities是用来写css原生的)

    - 打开预构建的 `css / style.css` 文件并在 `@utilities` 指令下面添加以下内容。

      ```css
      @tailwind base;
      
      @tailwind components;
      
      @tailwind utilities;
      
      @layer utilities {
          .btn1 { width: 100px; height: 100px; background-color:red; display: inline-block}
      }
      ```

      

  - 伪类设置悬停

    - 了解如何通过悬停在 Tailwind 中执行伪类，以在光标悬停在按钮上时更改按钮的颜色。如果要更改颜色，请在悬停后设置颜色，设置将可以体现出来。

    - ```html
      <button class="bg-red-700 btn hover:bg-red-500">啦啦啦</button>
      ```

    - 使用方法为：`hover：悬停样式`

  - 过渡设置

    - 我确认通过设置伪类的悬停可以在光标移到按钮上时更改按钮的颜色。当光标悬停在按钮上时，你可以看到颜色。你可以通过使用过渡慢慢改变按钮的颜色。

    - 下面通过设置duration-1000，颜色会在1秒内缓慢变化。

      ```html
      <button class="bg-indigo-700 font-semibold text-white py-2 px-4 rounded hover:bg-red-700 duration-1000">aaa</button>
      ```

    -   使用方法：**持续时间的多个值从duration-75 到duration-1000 注册。**

  - 变换设置

    - 如果你想让按钮本身变大并通过悬停更改按钮的颜色，您可以使用`transform` 和`scale`来实现。

    - ```html
      <button class="bg-indigo-700 font-semibold text-white py-2 px-4 rounded transform hover:scale-110 hover:bg-red-700 duration-1000">ddd</button>
      ```

  - 群组设置

    - 到目前为止的hover设置中，当光标经过目标元素时，hover的变化就会发生在元素上，但是**在group设置中，当光标经过父元素时，设置hover的子元素中就可以呈现hover效果。**

    - 在下面的示例中，当光标经过设置了 group 的父元素时，由于为子元素设置的悬停设置，一个 p 标签元素的文本颜色变为红色，另一个变为蓝色。

      ```html
      <div class="group m-10 p-10 border hover:bg-gray-100">
        <p class="font-black group-hover:text-red-900">aaa</p>
        <p class="font-black group-hover:text-blue-900">bbb</p>
      </div>
      ```

  - 动画设置

    - 只需将 `animate-bounce` 和 `animate-pulse` 设置为 class，就可以轻松设置动画，而无需设置复杂的 CSS。

    

- 在vue中使用tailwind css

  - 在vue的根目录下，安装 Tailwind 及其依赖项（PostCSS & auto-prefixer）。

    `npm install -D tailwindcss postcss autoprefixer`

  - 生成Tailwind CSS配置文件

    `npx tailwindcss init -p`

  - 在配置文件`content`中添加所有模板文件的路径

    ```js
    //tailwind.config.js
    module.exports = {
      content: [
        "./index.html",
        "./src/**/*.{vue,js,ts,jsx,tsx}",
      ],
      theme: {
        extend: {},
      },
      plugins: [],
    }
    ```

  - 创建一个`tailwindcss.css`样式文件，用于初始化并引入`tailwindcss`的基础样式

    ```css
    /* /src/css/tailwindcss.css*/
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```

  - 导入`css/tailwindcss.css`到`main.js`，这样就让你的项目拥有了`Tailwind CSS`

    ```js
    // src/main.js
    import { createApp } from 'vue'
    import App from './App.vue'
    import "./css/tailwindcss.css"
    
    createApp(App).mount('#app')
    ```

- 

