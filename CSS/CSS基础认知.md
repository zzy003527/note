## CSS基础认知

- CSS（Cascading style sheets）：**层叠样式表**

- CSS的作用是什么？

  - 给页面中的HTML标签设置样式

- CSS语法规则

  - 写在哪里？

    CSS写在style标签中，style标签一般写在head标签里面，title标签下面

  - 怎么写？

    ​              ![QQ图片20211018140947](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211018140947.png)

    

- CSS初体验

  - 常见属性：

  ​       ![QQ图片20211018141155](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211018141155.png)![QQ图片20211018141213](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211018141213.png)

  - 注意点：
    - CSS标点符号都是**英文状态**下的
    - 每一个样式键值对写完之后，最后需要写分号

  ​              

- CSS引入方式

  - 内嵌式：CSS写在style标签中

    - 提示：style标签虽然可以写在页面任意位置，但是通常约定写在**head标签**中

  - 外联式：CSS写在一个单独的.css文件中

    - 提示：需要通过link标签在网页中引入

  - 行内式：CSS写在标签的style属性中

    - 提示：基础班不推荐使用，之后会配合js使用

  - 小结：

    | 引入方式 | 书写位置                                 | 作用范围 | 使用场景   |
    | -------- | ---------------------------------------- | -------- | ---------- |
    | 内嵌式   | CSS写在style标签中                       | 当前页面 | 小案例     |
    | 外联式   | CSS写在单独的css文件中，通过link标签引入 | 多个页面 | 项目中     |
    | 行内式   | CSS写在标签的style属性中                 | 当前标签 | 配合js使用 |

