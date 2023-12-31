## CSS三大特性

1.继承性

- 继承性的介绍

  - 特性：子元素有默认继承父元素样式的特点（子承父业）
  - 可以继承的常见属性：
    - color
    - font-style、font-weight、font-size、font-family
    - text-indent、text-align
    - line-height
  - 注意点：可以通过调试工具判断样式是否可以继承

- 继承的应用

  - 好处：可以在一定程度上减少代码
  - 常见应用场景：
    - **可以直接给ul设置list-style：none属性，从而去除列表默认的小圆点样式**
    - 直接给body标签设置统一的font-size，从而统一不同浏览器默认文字大小

- 继承失效的特殊情况

  - 如果元素有浏览器默认样式，此时继承性依然存在，但是优先显示浏览器的默认样式
  - a标签的color会继承失效
    - 其实color属性继承下来了，但是被浏览器默认设置的样式给覆盖掉了
  - h系列标签的font-size会继承失效
    - 其实font-size属性继承下来了，但是被浏览器默认设置的样式给覆盖掉了
  - div的高度不能继承，但是宽度有类似于继承的效果
    - 宽度属性不能继承，但是div有独占一行的特性

  

2.层叠性

- 层叠性的介绍

  - 特性：
    - 给同一个标签设置不同的样式--->此时样式会层层叠加--->会共同作用在标签上
    - 给同一个标签设置相同的样式--->此时样式会层叠覆盖--->最终写在最后的样式会生效
  - 注意点：**当样式冲突时，<u>只有当选择器优先级相同时</u>，才能通过层叠性判断结果**

  

3.优先级

- 优先级的介绍

  - 特性：不同选择器具有不同的优先级，优先级高的选择器样式会覆盖优先级低选择器样式
  - 优先级公式：**<u>继承<通配符选择器<标签选择器/伪元素<类选择器/属性选择器/伪类<id选择器<行内样式<！important</u>**
    - （加在所需属性之后，如：color：purplr  ！important）
  - 注意点：
    - ！important写在属性值的后面，分号的前面！
    - ！important不能提升继承的优先级，**只要是继承优先级就最低**
    - 实际开发中不建议使用！important
  
- 权重叠加计算

  - 场景：如果是复合选择器，此时需要通过权重叠加计算方法，判断最终哪个选择器优先级最高会生效

  - 权重叠加计算公式：（每一级之间不存在进位）

    ![QQ图片20211127110331](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211127110331.png)

  - 比较规则：

    - 先比较第一级数字，如果比较出来了，之后的统统不看
    - 如果第一级数字相同，此时再去比较第二级数字，如果比较出来了，之后的统统不看
    - 如果最终所有数字都相同，表示优先级相同，此时比较层叠性（谁在下面就表示谁）

  - 注意点：**！important如果不继承，则权重最高**

- 权重叠加计算案例

  - 权重计算题解题步骤：
    - 先判断选择器是否能直接选中标签，如果不能直接选择--->是继承，优先级最低--->直接pass
    - 通过权重计算公式，判断谁权重最高
    - 继承中离得越近，优先级越高
  - 注意点：实际开发中选择选择标签需要精准，尽量避免多个选择器同时选中一个标签的情况
  - 

















