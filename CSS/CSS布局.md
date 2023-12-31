## CSS布局

- 结构伪类选择器

  - 作用与优势

    - 作用：根据元素在HTML中的结构关系查找元素
    - 优势：减少对于HTML中类的依赖，有利于保持代码整洁
    - 场景：常用于查找某父级选择器中的子元素

  - 选择器

    | 选择器                   | 说明                                           |
    | ------------------------ | ---------------------------------------------- |
    | E:first-child{}          | 匹配父元素中的第一个子元素，并且是E元素        |
    | E：last-child{}          | 匹配父元素中的最后一个子元素，并且是E元素      |
    | E：nth-child（n）{}      | 匹配父元素中第n个元素，并且是E元素             |
    | E：nth-last-child（n）{} | 匹配父元素中倒数第n个元素，并且是E元素         |
    | E：nth-of-type（n）{}    | 只在父元素的同类型（E）子元素范围中，匹配第n个 |

  - 区别：

    - ：nth-child--->直接在所有孩子中数个数
    - ：nth-of-type--->先通过该**类型**找到复合的一堆子元素，然后在这一堆子元素中数个数

  - n的注意点：

    - n为：0，1，2，3，4，5，6，

    - 通过n可以组成常见公式

      | 功能            | 公式            |
      | --------------- | --------------- |
      | 偶数            | 2n、even        |
      | 奇数            | 2n+1、2n-1、odd |
      | 找到前5个       | -n+5            |
      | 找到从第5个往后 | n+5             |

      

  - 结构伪类选择器的易错点

    ```css
    <style>
            /* 错误的写法 */
             li a:first-child {
            }
            /* 正确的写法 */
            li:first-child a{
                color:red
            }
        </style>
    
        <ul>
            <li><a href="">我是第1个a</a></li>
            <li><a href="">我是第2个a</a></li>
            <li><a href="">我是第3个a</a></li>
            <li><a href="">我是第4个a</a></li>
            <li><a href="">我是第5个a</a></li>
            <li><a href="">我是第6个a</a></li>
        </ul>
    ```

    

- 伪元素

  - 伪元素：一般页面中的非主体内容可以使用伪元素

  - 区别：

    - 元素：HTML设置的标签
    - 伪元素：由CSS模拟出的效果

  - 种类：

    | 伪元素    | 作用                             |
    | --------- | -------------------------------- |
    | : :before | 在父元素内容的最前添加一个伪元素 |
    | : :after  | 在父元素内容的最后添加一个伪元素 |

  - 注意点：

    - **必须设置content属性才能生效**
    - **伪元素默认是行内元素**

- 标准流

  - 标准流：又称**文档流**，是浏览器在渲染显示网页内容时默认采用的一套排版规则，规定了应该以何种方式排列元素
  - 常见标准流排版规则：
    - 块级元素：从上往下，**垂直布局**，独占一行
    - 行内元素或行内块元素：从左往右，**水平布局**，空间不够自动折行







- 浮动的作用

  - 早期的作用：图文环绕

    ![QQ图片20211127202800](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211127202800.png)

  - 现在的作用：网页布局

    - 场景：让垂直布局的盒子变成水平布局，如：一个在左，一个在右

      ![QQ图片20211127202814](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211127202814.png)

    

- 浮动的代码

  - 属性名：float

  - 属性值：

    | 属性名 | 效果   |
    | ------ | ------ |
    | left   | 左浮动 |
    | right  | 右浮动 |

- 浮动的特点

  - 浮动元素会脱离标准流（简称：脱流），在标准流中不占位置
    - 相当于从地面飘到了空中
  - 浮动元素比标准流高半个级别，可以覆盖标准流中的元素    
  - 浮动找浮动，下一个浮动元素会在上一个浮动元素后面左右浮动
  - 浮动元素会受到上面元素边界的影响
  - 浮动元素有特殊的显示效果
    - 一行可以显示多个
    - 可以设置宽高
  - 注意点：浮动的元素不能通过text-align：center或者margin：0 auto，让浮动元素本身水平居中

- <u>**书写网页导航的基本写法**</u>

  - **清除默认的margin和padding**
  - **找到ul，去除小圆点**
  - **找到li标签，设置浮动让li在一行中显示**
  - **找到a标签，设置宽高-->a标签默认是行内元素，默认不能设置宽高，故：**
    - **方法一：给a标签设置display：inline-block**
    - **方法二：给a标签设置display：block**
    - **方法三：给a设置float：left**

- 清除浮动的介绍

  - 含义：**清除浮动带来的影响**
    - 影响：如果子元素浮动了，此时子元素不能撑开标准流的块级父元素
  - 原因：子元素浮动后脱标-->不占位置
  - 目的：需要父元素有高度，从而不影响其他网页元素的布局

- 清除浮动的方法

  - 方法1：直接设置父元素高度

    - 特点：优点：坚定粗暴、方便

      ​            缺点：有些布局中不能固定父元素高度。如：新闻列表、京东推荐模块

  - 方法2：额外标签法

    - 操作：

      1.在父元素内容的最后添加一个块级元素

      2.给添加的块级元素设置**clear：both**

    - 特点：缺点：会在页面中添加额外的标签，会让页面的HTML结构变得复杂

  - 方法3：单伪元素清除法

    - 操作：用伪元素替代了额外标签

    - 基本写法：

      ```css
      .clearfix::after {
      content:'';
          display: block;
          clear:both;
      }
      ```

      

    - 补充写法：

      ```css
      .clearfix::after {
      content:'';
       display: block;
          clear:both;
      /*补充代码：在网页中看不到伪元素*/
          height:0;
          visibility:hidden;
      }
      ```

      

    - 特点：优点：项目中使用，直接给标签加类即可清除浮动

  - 方法4：双伪元素清除法

    - 操作：

      ```css
      .clearfix::before,
      .clearfix::after {
      content:"";
          display:table;
      }
      .clearfix::after {
      clear: both;
      }
      ```

    - 特点：优点：项目中使用，直接给标签加类即可清除浮动

    - **双伪元素清除法可以解决margin的塌陷问题**

  - 方法5：给父元素设置overflow：hidden

    - 操作：直接给父元素设置overflow：hidden
    - 特点：优点：方便





- BFC的介绍

  - 块格式化上下文（Block Formatting Context) : BFC

    - 是Web页面的可视CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域

  - 创建BFC方法：

    - html标签是BFC盒子
    - 浮动元素是BFC盒子
    - 行内块元素是BFC盒子
    - overflow属性取值不为visible。如：auto、hidden

  - BFC盒子常见特点：

    - BFC盒子会默认包裹住内部子元素（标准流、浮动）-->应用：清除浮动
    - BFC盒子本身与子元素之间不存在margin的塌陷现象-->应用：解决margin的塌陷

    

































































