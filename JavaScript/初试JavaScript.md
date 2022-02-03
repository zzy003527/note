## 初试JavaScript

- JavaScript是什么

  - JavaScript是一种运行在客户端的脚本语言（script是脚本的意思）
  - 脚本语言：不需要编译，运行过程中由js解释器（js引擎）逐行来进行解释并执行
  - 现在也可以基于Node.js技术进行服务器端编程

- 浏览器执行JS简介

  - 浏览器分成两部分，渲染引擎和JS引擎
  - 渲染引擎：用来解析HTML与CSS，俗称内核，比如chrome浏览器的blink，老版本的webkit
  - JS引擎：也成为JS解释器。用来读取网页中的JavaScript代码，对其处理后运行，比如chrome浏览器的V8

- JS的组成

  - ECMAScript（JavaScript语法）、DOM（页面文档对象模型）、BOM（浏览器对象模型）
  - ECMAScript规定了JS的编程语法和基础核心知识，是所有浏览器厂商共同遵守的一套JS语法工业标准
  - DOM（文档对象模型）：是W3C组织推荐的处理可扩展标记语言的标准编程接口。通过DOM提供的接口可以对页面上的各种元素进行操作（大小、位置、颜色等）
  - BOM（浏览器对象模型）：它提供了独立于内容的、可以于浏览器窗口进行互动的对象结构。通过BOM可以操作浏览器窗口，比如弹出框、控制浏览器跳转、获取分辨率等

- JS书写位置

  - JS有三种书写位置，分别为行内、内嵌和外部。

  - 行内式JS

    - ```javascript
      <input type="button" value="点我试试" onclick="alert('Hello World')"/>
      ```

    - 可以将单行或少量JS代码写在HTML标签的事件属性中（以on开头的属性），如：onclick

    - 注意单双引号的使用：在**HTML**中我们推荐使用**双引号**，**JS**中我们推荐使用**单引号**

    - 可读性差，在HTML中编写大量JS代码时，不方便阅读

    - 引号易错，引号多层嵌套匹配时，非常容易弄混

    - 特殊情况下使用

  - 内嵌JS 

    - ```javascript
       <script>
              alert('Hello World');
          </script>
      ```

    - 可以将多行JS代码写到<script>标签中

    - 内嵌JS 是学习时常用的方式

  - 外部JS文件

    - ```javascript
      <script src="my.js"></script>
      ```

    - 利于HTML页面代码结构化，把大段JS代码独立到HTML页面之外，既美观，也方便文件级别的复用

    - 引用外部JS文件的script标签中间不可以写代码

    - 适合于JS代码量较大的情况

- JavaScript注释

  - 单行注释：//            快捷键：ctrl+/
  - 多行注释：/*  */          快捷键：ctrl+shift+/

- JavaScript输入输出语句

  - | 方法             | 说明                           | 归属   |
    | ---------------- | ------------------------------ | ------ |
    | alert(msg)       | 浏览器弹出警示框               | 浏览器 |
    | console.log(msg) | 浏览器控制台打印输出信息       | 浏览器 |
    | prompt(info)     | 浏览器弹出输出框，用户可以输入 | 浏览器 |

    