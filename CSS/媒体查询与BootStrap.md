## 媒体查询与BootStrap

### 1.媒体查询

- 开发常用写法

  - 媒体特性常用写法

    - max-width（大于等于）
    - min-width（小于等于）

  - 书写顺序：

    - min-width：从小到大
    - max-width：从大到小

  - ```css
    @media （媒体特性） {
        选择器 {
                    样式
        }
    }
    ```

- 完整写法：

  - ```css
    @media 关键词 媒体类型 and （媒体特性） {CSS代码}
    ```

  - and（有两个条件时用来连接两个条件）

- 媒体类型

  - | 类型名称   | 值     | 描述                     |
    | ---------- | ------ | ------------------------ |
    | 屏幕       | screen | 带屏幕的设备             |
    | 打印预览   | print  | 打印预览模式             |
    | 阅读器     | speech | 屏幕阅读模式             |
    | 不区分类型 | all    | 默认值，包括以上三种情形 |

  - 媒体类型是用来**区分设备类型**的，如屏幕设备，打印设备等，其中手机、电脑、平板都属于屏幕设备

- 媒体特性

  - 媒体特性主要用来**描述媒体类型的具体特征**，如当前屏幕的宽高、分辨率、横屏或竖屏等

  - | 特性名称       | 属性                  | 值                                |
    | -------------- | --------------------- | --------------------------------- |
    | 视口的宽和高   | width、height         | 数值                              |
    | 视口最大宽或高 | max-width、max-height | 数值                              |
    | 视口最小宽或高 | min-width、min-height | 数值                              |
    | 屏幕方向       | orientation           | portrait：竖屏    landscape：横屏 |

    

- 基本语法

  - 外链式CSS引入

    ```css
    <link rel="stylesheet" media="逻辑符 媒体类型 and （媒体特性）" herf="xx.css">
    ```

    

### 2.BootStrap

- 简介

  - BootStrap是由Twitter公司开发维护的前端UI框架，它提供了大量编写好的CSS样式，允许开发者结合一定的HTML结构及JavaScript，快速编写功能完善的网页及常见交互效果
  - 中文官网：https：//www.bootcss.com/

- 使用步骤

  - **引入**：BootStrap提供的CSS代码

    ```css
    <link rel="stylesheet" href="./bootstrap-3.3.7/css/bootstrap.css">
    ```

  - **调用类**：使用BootStrap提供的样式

    - **container**：响应式布局版心类

- BootStrap栅格系统

  - 栅格化是指将整个网页的宽度分成若干等份

  - BootStrap3默认将网页分成**12等份**

    |              | 超小屏幕                          | 小屏幕       | 中等屏幕     | 大屏幕       |
    | ------------ | --------------------------------- | ------------ | ------------ | ------------ |
    | **响应断点** | **<768px**                        | **>=768px**  | **>=992px**  | **>=1200px** |
    | 别名         | xs                                | sm           | md           | lg           |
    | 容器宽度     | 100%                              | 750px        | 970px        | 1170px       |
    | **类前缀**   | **col-xs-*(  *代表的是所占份数)** | **col-sm-*** | **col-md-*** | **col-lg-*** |
    | 列数         | 12                                | 12           | 12           | 12           |
    | 列间隙       | 30px                              | 30px         | 30px         | 30px         |

  - **.container**是BootStrap中专门提供的类名，所有应用该类名的盒子，默认已被**指定宽度且居中**

  - **.container-fluid**也是BootStrap中专门提供的类名，所有应用该类名的盒子，**宽度均为100%**

  - 分别使用.row类名和.col类名定义栅格布局的行和列（可以消除container自带的间距）

  - 注意：

    - **container类自带内边距15px**
    - **row类自带外边距-15px**

- 全局样式

  - BootStrap手册用法

    - 网站首页-->BootStrap3中文文档-->**全局css样式**-->按**分类导航**查找目标类

  - 组件

    - BootStrap提供的常见功能，包含了HTML结构和CSS样式
    - 使用方法：引入BootStrap样式；复制结构，修改内容

  - Glyphicons字体图标

    - 使用步骤：

      1.HTML页面引入BootStrap样式文件

      2.空标签调用对应类名：Glyphicon；图标类

  - 插件使用步骤

    - 引入BootStrap样式

    - 引入js文件：jQuery.JS + BootStrap.min.js

      在body引入

      ```css
      <script src="./js/jquery.js"></script>
      <csript src="./bootstrap-3.4.1-dist/js/bootstrap.min.js"></script>
      ```

    - 复制HTML结构，并适当调整结构或内容

    