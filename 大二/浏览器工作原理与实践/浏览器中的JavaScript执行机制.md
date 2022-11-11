# 浏览器中的JavaScript执行机制

## 一.变量提升：JavaScript的代码是按顺序执行的吗

### 1.1 变量提升

- 在介绍变量提升之前，我们先通过下面这段代码，来看看什么是JavaScript中的声明和赋值。

  ```text
  var myname = '极客时间'
  ```

  这段代码你可以把它看成是两行代码组成的：

  ```text
  var myname    //声明部分
  myname = '极客时间'  //赋值部分
  ```

  - ![img](https://static001.geekbang.org/resource/image/ec/3c/ec882f2d9deec26ce168b409f274533c.png)

- 上面是变量的声明和赋值，那接下来我们再来看看函数的声明和赋值，结合下面这段代码：

  ```js
  function foo(){
    console.log('foo')
  }
  
  var bar = function(){
    console.log('bar')
  }
  ```

  - 第一个函数foo是一个完整的函数声明，也就是说没有涉及到赋值操作；第二个函数是先声明变量bar，再把`function(){console.log('bar')}`赋值给bar。

- **什么是变量提升**

  - **所谓的变量提升，是指在JavaScript代码执行过程中，JavaScript引擎把变量的声明部分和函数的声明部分提升到代码开头的“行为”。变量被提升后，会给变量设置默认值，这个默认值就是我们熟悉的undefined**。

  - 模拟执行：

    - ```js
      showName()
      console.log(myname)
      var myname = '极客时间'
      function showName() {
          console.log('函数showName被执行');
      }
      ```

    - 上面那段代码可看为：

      ```js
      /*
      * 变量提升部分
      */
      // 把变量 myname提升到开头，
      // 同时给myname赋值为undefined
      var myname = undefined
      // 把函数showName提升到开头
      function showName() {
          console.log('showName被调用');
      }
      
      /*
      * 可执行代码部分
      */
      showName()
      console.log(myname)
      // 去掉var声明部分，保留赋值语句
      myname = '极客时间'
      ```

      - ![img](https://static001.geekbang.org/resource/image/ce/d5/cefe564dbff729e735a834fd9e3bd0d5.png)
      - 从图中可以看出，对原来的代码主要做了两处调整：
        - 第一处是把声明的部分都提升到了代码开头，如变量`myname`和函数`showName`，并给变量设置默认值`undefined`；
        - 第二处是移除原本声明的变量和函数，如var myname = '极客时间'的语句，移除了`var`声明，整个移除`showName`的函数声明。
        - 通过这两步，就可以实现变量提升的效果。

      

    





### 1.2 JavaScript的执行流程

- 从概念的字面意义上来看，“变量提升”意味着变量和函数的声明会在物理层面移动到代码的最前面。
- 但，这并不准确。**实际上变量和函数声明在代码里的位置是不会改变的，而且是在编译阶段被JavaScript引擎放入内存中**。对，你没听错，**一段JavaScript代码在执行之前需要被JavaScript引擎编译，编译完成之后，才会进入执行阶段**。大致流程你可以参考下图：![img](https://static001.geekbang.org/resource/image/64/1e/649c6e3b5509ffd40e13ce9c91b3d91e.png)



#### 1.2.1 编译阶段

- 那么**编译阶段和变量提升存在什么关系**呢？

  - 可以将上面的代码分为两部分：

    - **第一部分：变量提升部分的代码。**

      ```js
      var myname = undefined
      function showName() {
          console.log('函数showName被执行');
      }
      ```

      **第二部分：执行部分的代码。**

      ```js
      showName()
      console.log(myname)
      myname = '极客时间'
      ```

  - 下面我们就可以把JavaScript的执行流程细化，如下图所示：

    ![img](https://static001.geekbang.org/resource/image/06/13/0655d18ec347a95dfbf843969a921a13.png)

  - 从上图可以看出，**输入一段代码，经过编译后，会生成两部分内容：执行上下文（Execution context）和可执行代码**。

  - **执行上下文是JavaScript执行一段代码时的运行环境**，比如调用一个函数，就会进入这个函数的执行上下文，确定该函数在执行期间用到的诸如this、变量、对象以及函数等。

  - **在执行上下文中存在一个变量环境的对象（Viriable Environment），该对象中保存了变量提升的内容**，比如上面代码中的变量myname和函数showName，都保存在该对象中。

  - 可以**简单地把变量环境对象看成是如下结构**

    ```text
    VariableEnvironment:
         myname -> undefined, 
         showName ->function : {console.log(myname)}
    ```

    

- 了解完变量环境对象的结构后，接下来，再结合下面这段代码来分析下是**如何生成变量环境对象**的。

  ```js
  showName()
  console.log(myname)
  var myname = '极客时间'
  function showName() {
      console.log('函数showName被执行');
  }
  ```

  - 我们可以一行一行来分析上述代码：

    - 第1行和第2行，由于**这两行代码不是声明操作，所以JavaScript引擎不会做任何处理**；
    - 第3行，由于这行是**经过var声明的**，因此**JavaScript引擎将在环境对象中创建一个名为myname的属性，并使用undefined对其初始化**；
    - 第4行，**JavaScript引擎发现了一个通过function定义的函数，所以它将函数定义存储到堆(HEAP）中，并在环境对象中创建一个showName的属性，然后将该属性值指向堆中函数的位置**。 **这样就生成了变量环境对象**。
    - 接下来**JavaScript引擎会把<u>声明以外</u>的代码编译为字节码**

    ```js
    showName()
    console.log(myname)
    myname = '极客时间'
    ```

  - 好了，现在有了执行上下文和可执行代码了，那么接下来就到了执行阶段了。







#### 1.2.2 执行阶段

- JavaScript引擎开始执行“可执行代码”，按照顺序一行一行地执行。下面我们就来一行一行分析下这个执行过程

  - 当**执行到showName函数时，JavaScript引擎便开始在变量环境对象中查找该函数**，由于变量环境对象中**存在该函数的引用**，所以JavaScript引擎**便开始执行该函数**，并输出“函数showName被执行”结果。
  - 接下来打印“myname”信息，JavaScript引擎继续**在变量环境对象中查找该对象**，由于变量环境存在myname变量，并且其值为undefined，所以这时候就输出undefined。
  - 接下来执行第3行，把“极客时间”赋给myname变量，**赋值后变量环境中的myname属性值改变为“极客时间”**，变量环境如下所示：

  ```text
  VariableEnvironment:
       myname -> "极客时间", 
       showName ->function : {console.log(myname)
  ```

- 以上就是一段代码的编译和执行流程。







### 1.3 代码中出现相同的变量或函数怎么办

- 在执行一段JavaScript代码之前，会编译代码，并将代码中的函数和变量保存到执行上下文的变量环境中，那么如果代码中出现了重名的函数或者变量，JavaScript引擎会如何处理？

- 我们先看下面这样一段代码

  ```js
  function showName() {
      console.log('极客邦');
  }
  showName();
  function showName() {
      console.log('极客时间');
  }
  showName(); 
  ```

  - 在上面代码中，我们先定义了一个showName的函数，该函数打印出来“极客邦”；然后调用showName，并定义了一个showName函数，这个showName函数打印出来的是“极客时间”；最后接着继续调用showName。那么你能分析出来这两次调用打印出来的值是什么吗？
  - **我们来分析下其完整执行流程**：
    - **首先是编译阶段**。遇到了第一个showName函数，会将该函数体存放到变量环境中。接下来是第二个showName函数，继续存放至变量环境中，但是变量环境中已经存在一个showName函数了，此时，**第二个showName函数会将第一个showName函数覆盖掉**。**这样变量环境中就只存在第二个showName函数了**。
    - **接下来是执行阶段**。先执行第一个showName函数，但由于是从变量环境中查找showName函数，而变量环境中只保存了第二个showName函数，所以最终调用的是第二个函数，打印的内容是“极客时间”。第二次执行showName函数也是走同样的流程，所以输出的结果也是“极客时间”
  - 综上所述，**一段代码如果定义了两个相同名字的函数，那么最终生效的是最后一个函数**







### 1.4 总结

- **JavaScript代码执行过程中，需要先做变量提升，而之所以需要实现变量提升，是因为JavaScript代码在执行之前需要先编译。** 在**编译阶段，变量和函数会被存放到变量环境中，变量的默认值会被设置为undefined**；在**代码执行阶段，JavaScript引擎会从变量环境中去查找自定义的变量和函数。**
- 如果在编译阶段，存在两个相同的函数，那么最终存放在变量环境中的是最后定义的那个，这是因为**后定义的会覆盖掉之前定义的**。
- JavaScript的执行机制：**先编译，再执行**。









## 二.调用栈：为什么JavaScript代码会出现栈溢出

### 1.1 什么是函数调用

- **哪些情况下代码才算是“一段”代码，才会在执行之前就进行编译并创建执行上下文**。一般说来，有这么三种情况

  - 当**JavaScript执行全局代码**的时候，会编译全局代码并**创建全局执行上下文**，而且**在整个页面的生存周期内，全局执行上下文只有一份。**
  - 当**调用一个函数的时候**，函数体内的代码会被编译，并**创建函数执行上下文**，一般情况下，**函数执行结束之后，创建的函数执行上下文会被销毁。**
  - 当**使用eval函数**的时候，eval的代码也会被编译，并**创建执行上下文**。

- 函数调用就是运行一个函数，具体使用方式是使用函数名称跟着一对小括号。下面我们看个简单的示例代码

  ```js
  var a = 2
  function add(){
  	var b = 10
  	return  a+b；
  }
  add()
  ```

  - 这段代码很简单，先是创建了一个add函数，接着在代码的最下面又调用了该函数。

  - 那么下面我们就利用这段简单的代码来解释下函数调用的过程：

    - 在执行到函数add()之前，JavaScript引擎会为上面这段代码创建全局执行上下文，包含了声明的函数和变量，你可以参考下图：

      ![img](https://static001.geekbang.org/resource/image/7f/da/7fa2ed18e702861890d767ea547533da.png)

    - 从图中可以看出，代码中全局变量和函数都保存在全局上下文的变量环境中。

    - 执行上下文准备好之后，便开始执行全局代码，当执行到add这儿时，JavaScript判断这是一个函数调用，那么将执行以下操作：

      - 首先，从全局执行上下文中，取出add函数代码。
      - 其次，对add函数的这段代码进行编译，并创建该函数的执行上下文和可执行代码。
      - 最后，执行代码，输出结果。

  - 完整流程：

    - ![img](https://static001.geekbang.org/resource/image/53/ca/537efd9e96771dc50737117e615533ca.png)
    - 就这样，当**执行到add函数**的时候，我们就**有了两个执行上下文了——全局执行上下文和add函数的执行上下文**。
    - 也就是说**在执行JavaScript时，可能会存在多个执行上下文**，那么JavaScript引擎是如何管理这些执行上下文的呢？
      - 答案是通过一种叫**栈的数据结构来管理的**。那什么是栈呢？它又是如何管理这些执行上下文呢？

    





### 1.2 什么是JavaScript的调用栈

- JavaScript引擎正是利用栈的这种结构来管理执行上下文的。**在执行上下文创建好后，JavaScript引擎会将执行上下文压入栈中，通常把这种用来<u>管理执行上下文的栈称为执行上下文栈，又称调用栈</u>。**

- 下面我们再来看段稍微复杂点的示例代码：

  ```js
  var a = 2
  function add(b,c){
    return b+c
  }
  function addAll(b,c){
  var d = 10
  result = add(b,c)
  return  a+result+d
  }
  addAll(3,6)
  ```

  - 在上面这段代码中，你可以看到它是在addAll函数中调用了add函数，那**在整个代码的执行过程中，调用栈是怎么变化的**呢？

  - **第一步，创建全局上下文，并将其压入栈底**。

    - 如下图所示

      ![img](https://static001.geekbang.org/resource/image/a5/1d/a5d7ec1f8f296412acc045835b85431d.png)

    - 从图中可以看出，**变量a、函数add和addAll都保存到了全局上下文**的变量环境对象中。

    - 全局执行上下文压入到调用栈后，JavaScript引擎便开始执行全局代码了。首先会执行a=2的赋值操作，执行该语句会将全局上下文变量环境中a的值设置为2。设置后的全局上下文的状态如下图所示：![img](https://static001.geekbang.org/resource/image/1d/1d/1d50269dbc5b4c69f83662ecdd977b1d.png)

  - 接下来，**第二步是调用addAll函数**。

    - 当**调用该函数时，JavaScript引擎会编译该函数，并为其创建一个执行上下文，最后还将该函数的执行上下文压入栈中**，如下图所示：

      ![img](https://static001.geekbang.org/resource/image/7d/52/7d6c4c45db4ef9b900678092e6c53652.png)

    - addAll函数的执行上下文创建好之后，便进入了函数代码的执行阶段了，这里先执行的是d=10的赋值操作，执行语句会将addAll函数执行上下文中的d由undefined变成了10。

  - 然后接着往下执行，**第三步，当执行到add函数调用语句时，同样会为其创建执行上下文，并将其压入调用栈**

    - 如下图所示：

      ![img](https://static001.geekbang.org/resource/image/cc/37/ccfe41d906040031a7df1e4f1bce5837.png)

    - 当**add函数返回时，该函数的执行上下文就会从栈顶弹出**，并将result的值设置为add函数的返回值，也就是9。如下图所示：

      ![img](https://static001.geekbang.org/resource/image/03/96/03ca801a5372f941bf17d6088fee0f96.png)

    - 紧接着**addAll执行最后一个相加操作后并返回，addAll的执行上下文也会从栈顶部弹出**，此时调用栈中就只剩下全局上下文了。最终如下图所示：

      ![img](https://static001.geekbang.org/resource/image/d0/7b/d0ac1d6e77735338fa97cc9a3f6c717b.png)

    - 至此，整个JavaScript流程执行结束了。

- **调用栈是JavaScript引擎追踪函数执行的一个机制**，当一次有多个函数被调用时，通过调用栈就能够追踪到哪个函数正在被执行以及各函数之间的调用关系。







### 1.3 在开发中，如何利用好调用栈

- 如何利用浏览器查看调用栈的信息

  - 当你执行一段复杂的代码时，你可能很难从代码文件中分析其调用关系，这时候你**可以在你想要查看的函数中加入断点，然后当执行到该函数时，就可以查看该函数的调用栈了。**

  - 这么说可能有点抽象，这里我们拿上面的那段代码做个演示，你可以**打开“开发者工具”，点击“Source”标签，选择JavaScript代码的页面，然后在第3行加上断点，并刷新页面。**你可以看到执行到add函数时，执行流程就暂停了，这时**可以通过右边“call stack”来查看当前的调用栈的情况**，如下图：

    ![img](https://static001.geekbang.org/resource/image/c0/a2/c0d303a289a535b87a6c445ba7f34fa2.png)

    - 从图中可以看出，右边的“call stack”下面显示出来了函数的调用关系：栈的最底部是anonymous，也就是全局的函数入口；中间是addAll函数；顶部是add函数。这就清晰地反映了函数的调用关系，所以在分析复杂结构代码，或者检查Bug时，调用栈都是非常有用的。

  - **除了通过断点来查看调用栈，你还可以使用`console.trace()`来输出当前的函数调用关系**，比如在示例代码中的add函数里面加上了`console.trace()`，你就可以看到控制台输出的结果，如下图：

    ![img](https://static001.geekbang.org/resource/image/ab/ce/abfba06cd23a7704a6eb148cff443ece.png)

    

- **栈溢出（Stack Overflow）**

  - **调用栈是一种用来管理执行上下文的数据结构，符合后进先出的规则。**

  - 注意，**调用栈是有大小的**，当入栈的执行上下文超过一定数目，JavaScript引擎就会报错，我们把这种错误叫做栈溢出。

    特别是在写递归代码的时候，就很容易出现**栈溢出**的情况。比如下面这段代码：

    ```js
    function division(a,b){
        return division(a,b)
    }
    console.log(division(1,2))
    ```

  - 当执行时，就会抛出栈溢出错误，如下图：![img](https://static001.geekbang.org/resource/image/b4/4d/b4f7196077d9ef4eac1ca6a279f2054d.png)

    - 从上图你可以看到，抛出的**错误信息为：超过了最大栈调用大小**（Maximum call stack size exceeded）。
    - 那为什么会出现这个问题呢？
      - 这是因为当JavaScript引擎开始执行这段代码时，它首先调用函数division，并创建执行上下文，压入栈中；然而，这个函数是递归的，并且没有任何终止条件，所以它会**一直创建新的函数执行上下文，并反复将其压入栈中，但栈是有容量限制的，超过最大数量后就会出现栈溢出的错误**。

  - 理解了栈溢出原因后，就可以使用一些方法来避免或者解决栈溢出的问题，比如把递归调用的形式改造成其他形式，或者使用加入定时器的方法来把当前任务拆分为其他很多小任务。

    

- 总结

  - 每调用一个函数，JavaScript引擎会为其创建执行上下文，并把该执行上下文压入调用栈，然后JavaScript引擎开始执行函数代码。
  - 如果在一个函数A中调用了另外一个函数B，那么JavaScript引擎会为B函数创建执行上下文，并将B函数的执行上下文压入栈顶。
  - 当前函数执行完毕后，JavaScript引擎会将该函数的执行上下文弹出栈。
  - 当分配的调用栈空间被占满时，会引发“堆栈溢出”问题。













## 三.块级作用域：var缺陷以及为什么要引入let和const

### 3.1 作用域（scope）

- **为什么`JavaScript`中会存在变量提升这个特性，而其他语言似乎都没有这个特性**呢？要讲清楚这个问题，我们就得先从作用域讲起

  - **作用域是指在程序中定义变量的区域，该位置决定了变量的生命周期。通俗地理解，作用域就是变量与函数的可访问范围，即作用域控制着变量和函数的可见性和生命周期**

  - 在ES6之前，ES的作用域只有两种：**全局作用域和函数作用域**。

    - **全局作用域中的对象在代码中的任何地方都能访问**，其生命周期伴随着页面的生命周期。
    - **函数作用域就是在函数内部定义的变量或者函数，并且定义的变量或者函数只能在函数内部被访问**。函数执行结束之后，函数内部定义的变量会被销毁。

  - 在ES6之前，JavaScript只支持这两种作用域，相较而言，**其他语言则都普遍支持块级作用域**。**块级作用域就是使用一对大括号包裹的一段代码**，比如函数、判断语句、循环语句，甚至单独的一个{}都可以被看作是一个块级作用域。

    为了更好地理解块级作用域，可以参考下面的一些示例代码：

    ```js
    //if块
    if(1){}
    
    //while块
    while(1){}
    
    //函数块
    function foo(){
     
    //for循环块
    for(let i = 0; i<100; i++){}
    
    //单独一个块
    {}
    ```

  - **没有了块级作用域，再把作用域内部的变量统一提升无疑是最快速、最简单的设计**，不过这也直接**导致了函数中的变量无论是在哪里声明的，在编译阶段都会被提取到执行上下文的变量环境中，所以这些变量在整个函数体内部的任何地方都是能被访问的**，这也就是JavaScript中的变量提升。





### 3.2 变量提升所带来的问题

- **变量容易在不被察觉的情况下被覆盖掉**

  - 如下列代码：

    ```js
    var myname = "极客时间"
    function showName(){
      console.log(myname);
      if(0){
       var myname = "极客邦"
      }
      console.log(myname);
    }
    showName()
    ```

    - 执行这段代码，最后打印出来的是undefined

    - 最终的调用栈状态如下图所示：

      ![img](https://static001.geekbang.org/resource/image/94/c9/944aaeaeb9ee50feea3c7d218acdd5c9.png)

    - showName函数的执行上下文创建后，JavaScript引擎便开始执行showName函数内部的代码了。首先执行的是：

      ```text
      console.log(myname);
      ```

      执行这段代码需要使用变量myname，结合上面的调用栈状态图，你可以看到这里有两个myname变量：一个在全局执行上下文中，其值是“极客时间”；另外一个在showName函数的执行上下文中，其值是undefined。

    - 在函数执行过程中，**JavaScript会优先从当前的执行上下文中查找变量**，由于变量提升，当前的执行上下文中就包含了变量myname，而值是undefined，所以获取到的myname的值就是undefined。

  

- **本应销毁的变量没有被销毁**

  - 下面这段让人误解更大的代码：

    ```js
    function foo(){
      for (var i = 0; i < 7; i++) {
      }
      console.log(i); 
    }
    foo()
    ```

    - 如果你使用C语言或者其他的大部分语言实现类似代码，在for循环结束之后，i就已经被销毁了，但是在JavaScript代码中，i的值并未被销毁，所以最后打印出来的是7。
    - 这同样也是由变量提升而导致的，在创建执行上下文阶段，变量i就已经被提升了，所以当for循环结束之后，变量i并没有被销毁。

    









### 3.3 ES6是如何解决变量提升带来的缺陷

- 为了解决这些问题，ES6引入了let和const关键字，从而使JavaScript也能像其他语言一样拥有了块级作用域。

- 关于let和const的用法，你可以参考下面代码：

  ```js
  let x = 5
  const y = 6
  x = 7
  y = 9 //报错，const声明的变量不可以修改
  ```

  - 从这段代码可以看出来，两者之间的区别是，**使用let关键字声明的变量是可以被改变的，而使用const声明的变量其值是不可以被改变的**

- **通过实际的例子来分析下，ES6是如何通过块级作用域来解决上面的问题**的？

  - 参考下面这段存在变量提升的代码：

    ```js
    function varTest() {
      var x = 1;
      if (true) {
        var x = 2;  // 同样的变量!
        console.log(x);  // 2
      }
      console.log(x);  // 2
    }
    ```

    - 在这段代码中，有两个地方都定义了变量x，第一个地方在函数块的顶部，第二个地方在if块的内部，由于var的作用范围是整个函数，所以在编译阶段，会生成如下的执行上下文：

      ![img](https://static001.geekbang.org/resource/image/45/bf/4501368679083f3a8e1a9e4a8e316dbf.png)

    - 从执行上下文的变量环境中可以看出，最终**只生成了一个变量x，函数体内所有对x的赋值操作都会直接改变变量环境中的x值**。

    - 所以上述代码最后通过console.log(x)输出的是2，而对于相同逻辑的代码，其他语言最后一步输出的值应该是1，因为在if块里面的声明不应该影响到块外面的变量。

  - 只需要**把var关键字替换为let关键字**，改造后的代码如下

    ```js
    function letTest() {
      let x = 1;
      if (true) {
        let x = 2;  // 不同的变量
        console.log(x);  // 2
      }
      console.log(x);  // 1
    }
    ```

    - 执行这段代码，其输出结果就和我们的预期是一致的。
    - 这是因为**let关键字是支持块级作用域的**，所以在编译阶段，JavaScript引擎并不会把if块中通过let声明的变量存放到变量环境中，这也就意味着在if块通过let声明的关键字，并不会提升到全函数可见。所以在if块之内打印出来的值是2，跳出语块之后，打印出来的值就是1了。这种就非常符合我们的编程习惯了：**作用块内声明的变量不影响块外面的变量**。

    







### 3.4 JavaScript是如何支持块级作用域的

- JavaScript引擎是通过变量环境实现函数级作用域的，那么**ES6又是如何在函数级作用域的基础之上，实现对块级作用域的支持**呢？

  - 先看下面这段代码

    ```js
    function foo(){
        var a = 1
        let b = 2
        {
          let b = 3
          var c = 4
          let d = 5
          console.log(a)
          console.log(b)
        }
        console.log(b) 
        console.log(c)
        console.log(d)
    }   
    foo()
    ```

    - 当执行上面这段代码的时候，**JavaScript引擎会先对其进行编译并创建执行上下文**，然后再按照顺序执行代码。但是现在的情况有点不一样，我们引入了let关键字，let关键字会创建块级作用域，那么**let关键字是如何影响执行上下文**的呢？

- 分析上面这段代码的执行流程

  - **第一步是编译并创建执行上下文**，下面是我画出来的执行上下文示意图，你可以参考下

    ![img](https://static001.geekbang.org/resource/image/f9/67/f9f67f2f53437218baef9dc724bd4c67.png)

    - 通过上图，我们可以得出以下结论：
      - 函数内部**通过var声明的变量**，在编译阶段全都被存放到**变量环境**里面了。
      - **通过let声明的变量**，在编译阶段会被存放到**词法环境**（Lexical Environment）中。
      - **在函数的作用域内部，通过let声明的变量并没有被存放到词法环境中。**

  - 接下来，第二步继续执行代码，当执行到代码块里面时，变量环境中a的值已经被设置成了1，词法环境中b的值已经被设置成了2，这时候函数的执行上下文就如下图所示：

    ![img](https://static001.geekbang.org/resource/image/7e/fa/7e0f7bc362e0dea21d27dc5fb08d06fa.png)

    - 从图中可以看出，**当进入函数的作用域块时，作用域块中通过let声明的变量，会被存放在词法环境的一个单独的区域中，这个区域中的变量并不影响作用域块外面的变量**，比如在作用域外面声明了变量b，在该作用域块内部也声明了变量b，当执行到作用域内部时，它们都是独立的存在。
    - 其实，**在词法环境内部，维护了一个小型栈结构，栈底是函数最外层的变量，进入一个作用域块后，就会把该作用域块内部的变量压到栈顶**；**当作用域执行完成之后，该作用域的信息就会从栈顶弹出，这就是词法环境的结构**。需要注意下，我**这里所讲的变量是指通过let或者const声明的变量。**

  - 再接下来，当执行到作用域块中的console.log(a)这行代码时，就需要**在词法环境和变量环境中查找变量a的值**了，具体**查找方式是：沿着词法环境的栈顶向下查询，如果在词法环境中的某个块中查找到了，就直接返回给JavaScript引擎，如果没有查找到，那么继续在变量环境中查找。**

    这样一个变量查找过程就完成了，你可以参考下图：

    ![img](https://static001.geekbang.org/resource/image/06/08/06c06a756632acb12aa97b3be57bb908.png)

    - 从上图可以清晰地看出变量查找流程

  - **当作用域块执行结束之后，其内部定义的变量就会从词法环境的栈顶弹出**，最终执行上下文如下图所示：

    ![img](https://static001.geekbang.org/resource/image/d4/28/d4f99640d62feba4202aa072f6369d28.png)

    - **块级作用域就是通过词法环境的栈结构来实现的，而变量提升是通过变量环境来实现，通过这两者的结合，JavaScript引擎也就同时支持了变量提升和块级作用域了**

    











## 四.作用域链和闭包：代码中出现相同的变量，JavaScript引擎如何选择

- 看下面这段代码：

  ```js
  function bar() {
      console.log(myName)
  }
  function foo() {
      var myName = " 极客邦 "
      bar()
  }
  var myName = " 极客时间 "
  foo()
  ```

  - 这段代码中的 bar 函数和 foo 函数打印出来的内容是什么？这就要分析下这两段代码的执行流程。

  - 当这段代码执行到 bar 函数内部时，其调用栈的状态图如下所示：![image-20221112000116406](C:\Users\ZZY\AppData\Roaming\Typora\typora-user-images\image-20221112000116406.png)

  

  - 从图中可以看出，全局执行上下文和 foo 函数的执行上下文中都包含变量 myName，那 bar 函数里面 myName 的值到底该选择哪个呢？

  - 也许你的第一反应是按照调用栈的顺序来查找变量，查找方式如下：
    1. 先查找栈顶是否存在 myName 变量，但是这里没有，所以接着往下查找 foo 函数中的变量。
    2. 在 foo 函数中查找到了 myName 变量，这时候就使用 foo 函数中的 myName。

  - 如果按照这种方式来查找变量，那么最终执行 bar 函数打印出来的结果就应该是“极客邦”。但实际情况并非如此，如果你试着执行上述代码，你会发现**打印出来的结果是“极客时间”**。为什么会是这种情况呢？要解释清楚这个问题，那么你就需要先搞清楚作用域链了

  



### 4.1 作用域链

- 其实**在每个执行上下文的变量环境中，都包含了一个外部引用，用来指向外部的执行上下文，我们把这个外部引用称为outer**。
- 当一段代码使用了一个变量时，JavaScript 引擎**首先会在“当前的执行上下文”中查找该变量**， 比如上面那段代码在查找 myName 变量时，**如果在当前的变量环境中没有查找到，那么 JavaScript 引擎会继续在 <u>outer 所指向的执行上下文中查找</u>**。
- 看下面这张图：![image-20221112000536208](C:\Users\ZZY\AppData\Roaming\Typora\typora-user-images\image-20221112000536208.png)
  - 从图中可以看出，**bar 函数和 foo 函数的 outer 都是指向全局上下文的，这也就意味着如果在 bar 函数或者 foo 函数中使用了外部变量，那么 JavaScript 引擎会去全局执行上下文中查找**。我们把**这个查找的链条就称为作用域链**。
  - 但是是foo 函数调用的 bar 函数，那**为什么 bar 函数的外部引用是全局执行上下文，而不是 foo 函数的执行上下文**？
    - 要回答这个问题，你还需要知道什么是词法作用域。这是因为**在 JavaScript 执行过程中，其作用域链是由词法作用域决定的**







### 4.2 词法作用域

- **词法作用域就是指作用域是由代码中函数声明的位置来决定的**，所以**词法作用域是静态的作用域，通过它就能够预测代码在执行过程中如何查找标识符**
- 看下面这张图：![image-20221112000812179](C:\Users\ZZY\AppData\Roaming\Typora\typora-user-images\image-20221112000812179.png)
  - 从图中可以看出，**词法作用域就是根据代码的位置来决定的**，其中 main 函数包含了 bar 函数，bar 函数中包含了 foo 函数**，因为 JavaScript 作用域链是由词法作用域决定的**，所以整个词法作用域链的顺序是：**foo 函数作用域—>bar 函数作用域—>main 函数作用域—> 全局作用域。**
- 了解了词法作用域以及 JavaScript 中的作用域链，我们再回过头来看看上面的那个问题：在开头那段代码中，foo 函数调用了 bar 函数，那为什么 bar 函数的外部引用是全局执行上下文，而不是 foo 函数的执行上下文?
  - 这是因为根据词法作用域，foo 和 bar 的上级作用域都是全局作用域，所以如果 foo 或者 bar 函数使用了一个它们没有定义的变量，那么它们会到全局作用域去查找。也就是说，**词法作用域是代码阶段就决定好的，和函数是怎么调用的没有关系**

- 看下面这段代码：

  ```js
  function bar() {
      var myName = " 极客世界 "
      let test1 = 100
      if (1) {
          let myName = "Chrome 浏览器 "
          console.log(test)
      }
  }
  function foo() {
      var myName = " 极客邦 "
      let test = 2
      {
          let test = 3
          bar()
      }
  }
  var myName = " 极客时间 "
  let myAge = 10
  let test = 1
  foo()
  ```

  - 要想得出其执行结果，那接下来我们就得站在作用域链和词法环境的角度来分析下其执行过程。

  - 在上篇文章中我们已经介绍过了，ES6 是支持块级作用域的，当执行到代码块时，如果代码块中有 let 或者 const 声明的变量，那么变量就会存放到该函数的词法环境中。对于上面这段代码，当执行到 bar 函数内部的 if 语句块时，其调用栈的情况如下图所示：![image-20221112002821782](C:\Users\ZZY\AppData\Roaming\Typora\typora-user-images\image-20221112002821782.png)

  - 现在是执行到 bar 函数的 if 语块之内，需要打印出来变量 test，那么就需要查找到 test 变量的值，其**查找过程已经在上图中使用序号 1、2、3、4、5 标记出来了**。

  - 下面我就来解释下这个过程。**首先是在 bar 函数的执行上下文中查找，但因为 bar 函数的执行上下文中没有定义 test 变量，所以根据词法作用域的规则，下一步就在 bar 函数的外部作用域中查找，也就是全局作用域**。

    









### 4.3 闭包

- 结合下面这段代码来理解什么是闭包：

  ```js
  function foo() {
      var myName = " 极客时间 "
      let test1 = 1
      const test2 = 2
      var innerBar = {
          getName:function(){
              console.log(test1)
              return myName
          },
          setName:function(newName){
              myName = newName
          }
      }
      return innerBar
  }
  var bar = foo()
  bar.setName(" 极客邦 ")
  bar.getName()
  console.log(bar.getName())
  ```

  - 首先我们看看当**执行到 foo 函数内部的return innerBar这行代码时调用栈的情况**，你可以参考下图：![image-20221112003150732](C:\Users\ZZY\AppData\Roaming\Typora\typora-user-images\image-20221112003150732.png)

    - 从上面的代码可以看出，**innerBar 是一个对象，包含了 getName 和 setName 的两个方法**（通常我们把对象内部的函数称为方法）。你可以看到，这两个方法都是在 foo 函数内部定义的，并且这两个方法内部都使用了 myName 和 test1 两个变量。

  - 根据词法作用域的规则，**内部函数 getName 和 setName 总是可以访问它们的外部函数 foo 中的变量**，所以**当 innerBar 对象返回给全局变量 bar 时，虽然 foo 函数已经执行结束，但是 getName 和 setName 函数依然可以使用 foo 函数中的变量 myName 和 test1**。所以**当 foo 函数执行完成之后，其整个调用栈的状态如下图所示**：![image-20221112003504258](C:\Users\ZZY\AppData\Roaming\Typora\typora-user-images\image-20221112003504258.png)

    - 从上图可以看出，**foo 函数执行完成之后，其执行上下文从栈顶弹出了**，但是由于**返回的 setName 和 getName 方法中使用了 foo 函数内部的变量 myName 和 test1，所以这两个变量依然保存在内存中**。这像极了 setName 和 getName 方法背的一个专属背包，**无论在哪里调用了 setName 和 getName 方法，它们都会背着这个 foo 函数的专属背包**。
    - **之所以是专属背包，是因为除了 setName 和 getName 函数之外，其他任何地方都是无法访问该背包的，我们就可以把这个背包称为 foo 函数的闭包。**

  - 现在我们终于可以给闭包一个正式的定义了。**在 JavaScript 中，根据词法作用域的规则，内部函数总是可以访问其外部函数中声明的变量，当通过调用一个外部函数返回一个内部函数后，即使该外部函数已经执行结束了，但是内部函数引用外部函数的变量依然保存在内存中，我们就把这些变量的集合称为闭包。**比如外部函数是 foo，那么这些变量的集合就称为 foo 函数的闭包。

  - 那这些闭包是如何使用的呢？当执行到 `bar.setName` 方法中的`myName = "极客邦"`这句代码时，JavaScript 引擎会沿着“`当前执行上下文–>foo 函数闭包–> 全局执行上下文`”的顺序来查找 myName 变量，可以参考下面的调用栈状态图：![image-20221112004019472](C:\Users\ZZY\AppData\Roaming\Typora\typora-user-images\image-20221112004019472.png)

    - 从图中可以看出，setName 的执行上下文中没有 myName 变量，foo 函数的闭包中包含了变量 myName，所以调用 setName 时，会修改 foo 闭包中的 myName 变量的值。
    - 同样的流程，当调用 bar.getName 的时候，所访问的变量 myName 也是位于 foo 函数闭包中的。

    

- 可以通过“开发者工具”来看看闭包的情况，打开 Chrome 的“开发者工具”，在 bar 函数任意地方打上断点，然后刷新页面，可以看到如下内容：![image-20221112004150401](C:\Users\ZZY\AppData\Roaming\Typora\typora-user-images\image-20221112004150401.png)

  - 从图中可以看出来，当调用 bar.getName 的时候，**右边 Scope 项就体现出了作用域链的情况**：**Local 就是当前的 getName 函数的作用域**，Closure(foo) 是指 foo 函数的闭包，最下面的 Global 就是指全局作用域，从“Local–>Closure(foo)–>Global”就是一个完整的作用域链。
  - 所以说，以后也可以通过 Scope 来查看实际代码作用域链的情况，这样调试代码也会比较方便







### 4.4 闭包是怎么回收的

- 理解什么是闭包之后，接下来我们再来简单聊聊闭包是什么时候销毁的。因为如果闭包使用不正确，会很容易造成内存泄漏的，关注闭包是如何回收的能让你正确地使用闭包。
- **通常，如果引用闭包的函数是一个全局变量，那么闭包会一直存在直到页面关闭；但如果这个闭包以后不再使用的话，就会造成内存泄漏。**
- **如果引用闭包的函数是个局部变量，等函数销毁后，在下次 JavaScript 引擎执行垃圾回收时，判断闭包这块内容如果已经不再被使用了，那么 JavaScript 引擎的垃圾回收器就会回收这块内存。**
- 所以在使用闭包的时候，你要尽量注意一个原则：**如果该闭包会一直使用，那么它可以作为全局变量而存在；但如果使用频率不高，而且占用内存又比较大的话，那就尽量让它成为一个局部变量。**











## 五. this：从JavaScript执行上下文视角讲this

### 5.1 JavaScript 中的 this 是什么

- 执行上下文中包含了变量环境、词法环境、外部环境，但其实还有一个 this 没有提及，具体可以参考下图：

  ![img](https://blog.poetries.top/img/static/gitee/2019/11/1.png)

  - 从图中可以看出，**this 是和执行上下文绑定的，也就是说每个执行上下文中都有一个 this**。前面提到过，执**行上下文主要分为三种——全局执行上下文、函数执行上下文和 eval 执行上下文，所以对应的 this 也只有这三种——全局执行上下文中的 this、函数中的 this 和 eval 中的 this。**
  - 那么接下来就重点讲解下全局执行上下文中的 this和函数执行上下文中的 this。







### 5.2 全局执行上下文中的 this

- 首先我们来看看全局执行上下文中的 this 是什么。
- 可以在控制台中输入`console.log(this)`来打印出来全局执行上下文中的 this，最终输出的是 window 对象。
- 所以可以得出这样一个结论：**全局执行上下文中的 this 是指向 window 对象的**。**这也是 this 和作用域链的唯一交点，作用域链的最底端包含了 window 对象，全局执行上下文中的 this 也是指向 window 对象**





### 5.3 函数执行上下文中的 this

- 先看下面这段代码：

  ```js
  function foo(){
    console.log(this)
  }
  foo()
  ```

  - 我们在 foo 函数内部打印出来 this 值，执行这段代码，打印出来的也是 window 对象，这说明**在默认情况下调用一个函数，其执行上下文中的 this 也是指向 window 对象的**。

- 通常情况下，有下面三种方式来设置函数执行上下文中的 this 值

  1. **通过函数的 call 方法设置**

     - **可以通过函数的call方法来设置函数执行上下文的 this 指向**，比如下面这段代码，我们就并没有直接调用 foo 函数，而是调用了 foo 的 call 方法，并将 bar 对象作为 call 方法的参数

       ```js
       let bar = {
         myName : " 极客邦 ",
         test1 : 1
       }
       function foo(){
         this.myName = " 极客时间 "
       }
       foo.call(bar)
       console.log(bar)
       console.log(myName)
       ```

     - 执行这段代码，然后观察输出结果，你就能发现 **foo 函数内部的 this 已经指向了 bar 对象**，因为通过打印 bar 对象，可以看出 bar 的 myName 属性已经由“极客邦”变为“极客时间”了，同时在全局执行上下文中打印 myName，JavaScript 引擎提示该变量未定义。

     - 其实**除了 call 方法，你还可以使用bind和apply方法来设置函数执行上下文中的 this**

  2. **通过对象调用方法设置**

     - 要改变函数执行上下文中的 this 指向，除了通过函数的 call 方法来实现外，还**可以通过对象调用的方式**，比如下面这段代码：

       ```js
       var myObj = {
         name : " 极客时间 ", 
         showThis: function(){
           console.log(this)
         }
       }
       myObj.showThis()
       ```

       - 在这段代码中，我们定义了一个 myObj 对象，该对象是由一个 name 属性和一个 showThis 方法组成的，然后再通过 myObj 对象来调用 showThis 方法。执行这段代码，你可以看到，**最终输出的 this 值是指向 myObj 的**。

       - 所以，你可以得出这样的结论：**使用对象来调用其内部的一个方法，该方法的 this 是指向对象本身的。**
       - 其实，你也可以认为 JavaScript 引擎在执行myObject.showThis()时，将其转化为了：

       ```text
       myObj.showThis.call(myObj)
       ```

     - 接下来稍微改变下调用方式，把 showThis 赋给一个全局对象，然后再调用该对象，代码如下所示：

       ```js
       var myObj = {
         name : " 极客时间 ",
         showThis: function(){
           this.name = " 极客邦 "
           console.log(this)
         }
       }
       var foo = myObj;
       foo.showThis();
       ```

       - 执行这段代码，你会发现 **this 又指向了全局 window 对象。**

       - 所以通过以上两个例子的对比，你可以得出下面这样两个结论：

         - **在全局环境中调用一个函数，函数内部的 this 指向的是全局变量 window。**

         - **通过一个对象来调用其内部的一个方法，该方法的执行上下文中的 this 指向对象本身。**

  3. **通过构造函数中设置**

     - 可以像这样**设置构造函数中的 this**，如下面的示例代码

       ```js
       function CreateObj(){
         this.name = " 极客时间 "
       }
       var myObj = new CreateObj()
       ```

     - 在这段代码中，我们**使用 new 创建了对象 myObj**，那你知道此时的构造函数 CreateObj 中的 this 到底指向了谁吗？

       其实，当**执行 `new CreateObj()` 的时候，JavaScript 引擎做了如下四件事**：

       - 首先创建了一个空对象 tempObj；
       - 接着调用 CreateObj.call 方法，并将 tempObj 作为 call 方法的参数，这样**当 CreateObj 的执行上下文创建时，它的 this 就指向了 tempObj 对象**；
       - 然后执行 CreateObj 函数，此时的 **CreateObj 函数执行上下文中的 this 指向了 tempObj 对象**；
       - 最后返回 tempObj 对象(赋值给myObj）。

     - 可以用代码来演示下

       ```js
       var tempObj = {}
       CreateObj.call(tempObj)
       return tempObj
       ```

       - 这样，我们就**通过 new 关键字构建好了一个新对象，并且构造函数中的 this 其实就是新对象本身**

       





### 5.4  this 的设计缺陷以及应对方案

- **嵌套函数中的 this 不会从外层函数中继承**

  - 结合下面这样一段代码来分析下：

    ```js
    var myObj = {
      name : " 极客时间 ", 
      showThis: function(){
        console.log(this)
        function bar(){console.log(this)}
        bar()
      }
    }
    myObj.showThis()
    ```

    - 我们在这段代码的 showThis 方法里面添加了一个 bar 方法，然后接着在 showThis 函数中调用了 bar 函数，那么现在的问题是：bar 函数中的 this 是什么？

    - 如果你是刚接触 JavaScript，那么你可能会很自然地觉得，bar 中的 this 应该和其外层 showThis 函数中的 this 是一致的，都是指向 myObj 对象的，这很符合人的直觉。但实际情况却并非如此，执行这段代码后，你会发现**函数 bar 中的 this 指向的是全局 window 对象**，**而函数 showThis 中的 this 指向的是 myObj 对象**。这就是 JavaScript 中非常容易让人迷惑的地方之一，也是很多问题的源头。

  - 可以通过一个小技巧来解决这个问题，比如在 showThis 函数中声明一个变量 self 用来保存 this，然后在 bar 函数中使用 self，代码如下所示：

    ```js
    var myObj = {
      name : " 极客时间 ", 
      showThis: function(){
        console.log(this)
        var self = this
        function bar(){
          self.name = " 极客邦 "
        }
        bar()
      }
    }
    myObj.showThis()
    console.log(myObj.name)
    console.log(window.name)
    ```

    - 执行这段代码，你可以看到它输出了我们想要的结果，**最终 myObj 中的 name 属性值变成了“极客邦”**。其实，**这个方法的的本质是把 this 体系转换为了作用域的体系**

  - 也可以使用 ES6 中的箭头函数来解决这个问题，结合下面代码：

    ```js
    var myObj = {
      name : " 极客时间 ", 
      showThis: function(){
        console.log(this)
        var bar = ()=>{
          this.name = " 极客邦 "
          console.log(this)
        }
        bar()
      }
    }
    myObj.showThis()
    console.log(myObj.name)
    console.log(window.name)
    ```

    - 执行这段代码，你会发现它也输出了我们想要的结果，也就是**箭头函数 bar 里面的 this 是指向 myObj 对象的**。这是因为 **ES6 中的箭头函数并不会创建其自身的执行上下文，所以箭头函数中的 this 取决于它的外部函数**。

    - 通过上面的讲解，你现在应该知道了 **this 没有作用域的限制**，这点和变量不一样，所以**嵌套函数不会从调用它的函数中继承 this**，这样会造成很多不符合直觉的代码。要解决这个问题，你可以有两种思路：

      - **第一种是把 this 保存为一个 self 变量，再利用变量的作用域机制传递给嵌套函数。**

      - **第二种是继续使用 this，但是要把嵌套函数改为箭头函数，因为箭头函数没有自己的执行上下文，所以它会继承调用函数中的 this**

    

- **普通函数中的 this 默认指向全局对象 window**

  - **在默认情况下调用一个函数，其执行上下文中的 this 是默认指向全局对象 window 的。**
  - 不过这个设计也是一种缺陷，因为在实际工作中，我们并不希望函数执行上下文中的 this 默认指向全局对象，因为这样会打破数据的边界，造成一些误操作。**如果要让函数执行上下文中的 this 指向某个对象，最好的方式是通过 call 方法来显示调用。**
  - 这个问题可以通过设置 JavaScript 的“严格模式”来解决。**在严格模式下，默认执行一个函数，其函数的执行上下文中的 this 值是 undefined**，这就解决上面的问题了

  

- 总结：

  - 在使用 this 时，为了避坑，你要谨记以下三点：

    - **当函数作为对象的方法调用时，函数中的 this 就是该对象；** 
    - **当函数被正常调用时，在严格模式下，this 值是 undefined，非严格模式下 this 指向的是全局对象 window；**
    -  **嵌套函数中的 this 不会继承外层函数的 this 值。 最后，我们还提了一下箭头函数，因为箭头函数没有自己的执行上下文，所以箭头函数的 this 就是它外层函数的 this。**

    
