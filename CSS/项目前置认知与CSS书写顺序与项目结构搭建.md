## 项目前置认知与CSS书写顺序与项目结构搭建

网页与网站的关系

- 在互联网中，网站类似于一本书，网页就是这本书的一页
- 多个同主题网页整合在一起，就称之为网站

网页与网站的概念

- 网页：网站的每一页。
- 网站：提供特定服务的一组网页集合

- DOCTYPE文档说明

  - ```html
    <!DOCTYPE html >
    ```

    文档类型说明，告诉浏览器该网页的HTML版本

- 网页语言

  - ```html
    <html lang="en">    </html>
    ```

    标识**网页**使用的语言

  - 作用：**搜索引擎归类+浏览器翻译**

  - 常见语言：**zh-CN**简体中文/**en**英文

- 字符编码

  - ```html
    <meta charset="UTF-8">
    ```

    标识**网页**使用的字符编码

  - 作用：保存和打开的字符编码需要统一设置，否则可能会出现乱码

  - 常见字符编码：

    - UTF-8：万国码，国际化的字符编码，收录了全球语言的文字
    - GB2312：6000+汉字
    - GBK：20000+汉字

  - 注意点：开发中**统一使用UTF-8字符编码**即可

- SEO简介

  - SEO（search Engine Optimization)：搜索引擎优化

  - 作用：让网站在搜索引擎上的排名靠前

  - 提升SEO的常见方法：

    - 竞价排名
    - 将网页制作成html后缀
    - 标签语义化（在合适的地方使用合适的标签）

  - SEO三大标签

    - title：网页标题标签

    - description：网页描述标签

    - keywords：网页关键词标签

      ![QQ图片20211201113256](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211201113256.png)

- ico图标设置

  - 场景：显示在标签页标题左侧的小图标，习惯使用.ico格式的图标

    ![QQ图片20211201113909](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211201113909.png)

  - 常见代码

    ```html
    <link rel="shortcut icon"  href="ico图标路径" type="image/x-icon">
    ```

- 版心的介绍

  - 场景：把页面的主体内容约束在网页中间

  - 作用：让不同大小的屏幕都能看到页面的主体内容

  - 代码

    ```html
    .container {
    width:1240px;
    margin: 0 auto;
    }
    ```

  - 注意点：版心类名常用：container、wrapper、w等





- CSS书写顺序

  - 衡量程序员的能力，除了要看实现业务需求的能力，还要看平时书写代码的规范（专业性）

  - 不同的CSS书写顺序会影响浏览器的渲染性能，推荐前端工程师使用专业的书写顺序习惯

    | 顺序 | 类别          | 属性                                                  |
    | ---- | ------------- | ----------------------------------------------------- |
    | 1    | 布局属性      | display、position、float、clear、visibility、overflow |
    | 2    | 盒子模型+背景 | width、height、margin、padding、border、background    |
    | 3    | 文本内容属性  | color、font、text-decoration、text-align、line-height |
    | 4    | 点缀属性      | cursor、border-radius、text-shadow、box-shadow        |

  - 开发中推荐**多用类+后代**，但不是层级越多越好，一个选择器中的类选择器的个数推荐**不要超过三个**








### 项目结构搭建

- 文件和目录准备
  - 新建项目文件夹**xtx-pc-client**，在Vscode中打开
    - 在实际开发中，项目文件夹不建议使用中文
    - 所有项目相关文件都保存在xtx-pc-client目录中
  - 复制favicon.ico到xtx-pc-client目录
    - 一般习惯将ico图标放在项目根目录
  - 复制images和uploads目录到xtx-pc-client目录中
    - images：存放网站**固定使用**的图片素材，如：logo、样式修饰图片等
    - uploads：存放网站**非固定使用**的图片素材，如：商品图片、宣传图片等
  - 新建index.hmtl在根目录
  - 新建css文件夹保存网站的样式，并新建以下CSS文件：
    - base.css：基础公共样式
    - common.css:该网站中多个网页相同模块的重复样式，如：头部、底部
    - index.css：首页样式
  - d
  - d
  - d
- d
- d
- d
- d
- d
- d
- d
- d
- d



































































































