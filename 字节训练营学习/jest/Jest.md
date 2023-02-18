# Jest

## 一、jest基本环境搭建

- ```js
  npm init
  npm install jest@24.8.0 -D
  ```





## 二、基本配置和测试覆盖率生成

- 单元测试：英文是(unit testing) 单,是指对软件中的最小可测试单元进行检查和验证。前端所说的单元测试就是对一个模块进行测试。也就是说前端测试的时候，你测试的东西一定是一个模块
- 集成测试：也叫组装测试或者联合测试。在单元测试的基础上，将所有模块按照涉及要求组装成为子系统或系统，进行集成测试。

- 基本配置

  - ```js
    npx jest --init
    ```

  - 下面有三个选项
    - Choose the test environment that will be used for testing ? 代码是运行在Node环境还是Web环境下？
    - Do you want Jest to add coverage reports ? 是否生成测试覆盖率文件？
    - Automatically clear mock calls and instrances between every test?是否需要在测试之后清楚模拟调用的一些东西？
  - 在这三个选项选择之后，你会发现你的工程根目录下多了一个`jest.config.js`的文件。打开文件你可以看到里边有很多`Jest`的配置项。

- 代码覆盖率：测试代码对功能性代码和业务代码做了百分之多少的测试

  - 即为`jest.config.js`中的`coverageDirectory: "coverage"`（后面的参数是生成的文件夹名）

  - ```js
    npx jest --coverage
    // 生成测试覆盖率
    ```

  - 可以打开`coverage-lcov-reporrt-index.html`文件，这时候就可以看到一个网页形式的，非常漂亮的测试覆盖率报告。







## 三、Jest中的匹配器

### 3.1 toBe()匹配器

- `toBe()`匹配器，是在工作中最常用的一种匹配器，简单的理解它就是相等。这个相当是等同于`===`的，也就是我们常说的严格相等。

- 为了更好的理解`toBe()`匹配器，我们新写一段代码，代码如下。

  ```js
  test('测试严格相等',()=>{
      const a = {number:'007'}   
      expect(a).toBe({number:'007'})
  }) 
  ```

- 写完后使用`npm run test`来进行测试。我们可以知道，这个不是完全相等的，引用地址不同，所以测试应该是`FAIL`。结果跟我们想象是一致的，通过这小段代码，可以很清楚的知道`toBe()`匹配器的作用





### 3.2 toEqual()匹配器

- 如果我想让上面的测试结果是正确的，这时候我需要使用什么匹配器那？那就是使用`toEqual()`匹配器。可以把它理解成只要内容相等，就可以通过测试，比如修改代码如下:

  ```js
  test('测试严格相等',()=>{
      const a = {number:'007'}   
      expect(a).toEqual({number:'007'})
  }) 
  ```

- 所以说当你不严格匹配但要求值相等时时就可以使用`toEqual（）`匹配器。





### 3.3 toBeNull()匹配器

- `toBeNul()`匹配器只匹配`null`值，需要注意的是**不匹配`undefined`的值**。我们复制上面的代码，然后把代码修改成如下形式：

  ```js
  test('toBeNull测试',()=>{
      const a = null   
      expect(a).toBeNull()
  }) 
  ```

- 但是如果我们把`a`的值改为`undefined`，测试用例就通过不了啦。





### 3.4 toBeUndifined()匹配器

- 那我们要匹配`undefined`时，就可以使用`toBeUndifined()`匹配器，比如写成如下代码。

  ```js
  test('toBeUndefined测试',()=>{
      const a = undefined   
      expect(a).toBeUndefined()
  }) 
  ```

- 如果我们把`undefined`改为空字符串也是没有办法通过测试的





### 3.5 toBeTruthy()匹配器

- 这个是`true`和`false`匹配器，就相当于判断真假的。比如说写下面这样一段代码:

  ```js
  test('toBeTruthy 测试',()=>{
      const a = 0
      expect(a).toBeTruthy()
  }) 
  ```

- 这样测试就过不去了，因为这里的0，如果判断真假时，就是false，所以无法通过。同样道理`null`也是无法通过的。 但是我们给个`1`或者`jspang`字符串，就可以通过测试了



### 3.6 toBeFalsy()匹配器

- 有真就有假，跟`toBeTruthy()`对应的就是`toBeFalsy`,这个匹配器只要是返回的`false`就可以通过测试。

  ```js
  test('toBeFalsy 测试',()=>{
      const a = 0
      expect(a).toBeFalsy()
  }) 
  ```



### 3.7 开启自动测试

- 每次修改测试用例，我们都手动输入`yarn test` ，这显得很lower。可以通过配置`package.json`文件来设置。修改如下：

  ```js
  {
    "name": "jesttest",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "jest --watchAll",
      "coverage":"jest --coverage"
    },
    "author": "",
    "license": "ISC",
    "devDependencies": {
      "jest": "^24.8.0"
    }
  }
  ```

- 修改保存后，我们在终端再次运行`yarn test`,这时候测试一次后，它并没有结束，而是等待测试文件的变化，变化后就会自动进行测试



### 3.8 toBeGreaterThan()匹配器

- 这个是用来作数字比较的，大于什么数值，只要大于传入的数值，就可以通过测试。我们来写一段代码来看一下。

  ```js
  test('toBeGreaterThan匹配器',()=>{
      const count = 10
      expect(count).toBeGreaterThan(9)
  })
  //10大于9，通过
  ```



### 3.9 toBeLessThan()匹配器

- `toBeLessThan`跟`toBeGreaterThan`相对应的，就是少于一个数字时，就可以通过测试。代码如下:

  ```js
  test('toBeLessThan匹配器',()=>{
      const count = 10
      expect(count).toBeLessThan(11)
  })
  ```

- 10比11小，所以测试用例顺利通过



### 3.10 toBeGreaterThanOrEqual()匹配器

- 当测试结果数据大于等于期待数字时，可以通过测试。

  - 比如下面的代码是没办法通过测试的。

  ```js
  test('toBeGreaterThan匹配器',()=>{
      const count = 10
      expect(count).toBeGreaterThan(10)
  })
  ```

- 但是我们使用了`toBeGreaterThanOrEqual()`就可以通过测试。

  ```js
  test('toBeGreaterThanOrEqual匹配器',()=>{
      const count = 10
      expect(count).toBeGreaterThanOrEqual(10)
  })
  ```



### 3.11 toBeLessThanOrEqual()匹配器

- 这个跟`toBeGreaterThanOrEqual()`相对应，所以就不做过多的介绍了



### 3.12 toBeCloseTo()匹配器

- 这个是可以自动消除`JavaScript`浮点精度错误的匹配器，举个例子，比如我们让`0.1`和`0.2`相加，这时候`js`得到的值应该是`0.30000000000004`,所以如果用`toEqual()`匹配器，测试用例会通过不了测试的。

- 比如我们把测试用例写成这样，就不会通过:

  ```js
  test('toEqual匹配器',()=>{
      const one = 0.1
      const two = 0.2
      expect(one + two).toEqual(0.3)
  })
  ```

- 这时候我们就需要使用`toBeCloseTo()`匹配器，可以顺利通过测试,代码如下：

  ```js
  test('toBeCloseTo匹配器',()=>{
      const one = 0.1
      const two = 0.2
      expect(one + two).toBeCloseTo(0.3)
  })
  ```

- 这样就可以顺利通过测试了，这就是它的作用。





### 3.13 toMatch()匹配器

- 字符串包含匹配器，比如象牙山洗脚城有三个美女：谢大脚、刘英和小红，这时候我们要看看字符串中有没有谢大脚就可以使用`toMatch()`匹配器。

  ```js
  test('toMatch匹配器',()=>{
      const str="谢大脚、刘英、小红"
      expect(str).toMatch('谢大脚')
  })
  ```

- 因为确实有“谢大脚”,所以就通过测试了，当然你也可以写正则表达式。

  ```js
  test('toMatch匹配器',()=>{
      const str="谢大脚、刘英、小红"
      expect(str).toMatch(/谢大脚/)
  })
  ```

- 我们可以开启`yarn test`就可以看到测试结果也是正确的



### 3.14 toContain()匹配器

- 刚才我们使用的只是一个字符串包换关系的匹配器，但是在工作中使用的多是数组，所以这里我们使用数组的匹配器`toContain()`。比如还是上面象牙山洗脚城的案例，我们就可以写成这样。

  ```js
  test('toContain匹配器',()=>{
      const arr=['谢大脚','刘英','小红']
      expect(arr).toContain('谢大脚')
  })
  ```

- 当然他也可以完美的兼容`set`的测试，比如把下面代码改为下面的方式。

  ```js
  test('toContain匹配器',()=>{
      const arr=['谢大脚','刘英','小红']
      const data = new Set(arr)
      expect(data).toContain('谢大脚')
  })
  ```





### 3.15 toThrow()匹配器

- 专门对异常进行处理的匹配器，可以检测一个方法会不会抛出异常。比如我们下面一段代码。

  ```js
  const throwNewErrorFunc = ()=>{
      throw new Error('this is a new error')
  }
  
  test('toThrow匹配器',()=>{
      expect(throwNewErrorFunc).toThrow()
  })
  ```

- 我们也可以对这个匹配器中加一些字符串，意思就是抛出的异常必须和字符串相对应。

  ```js
  const throwNewErrorFunc = ()=>{
      throw new Error('this is a new error')
  }
  
  test('toThrow匹配器',()=>{
      expect(throwNewErrorFunc).toThrow('this is a new error')
  })
  ```

- 如果字符串不匹配，也没办法通过异常测试



### 3.16 not匹配器

- `not`匹配器是`Jest`中比较特殊的匹配器，意思就是`相反`或者说`取反`.比如上面的例子，我们不希望方法抛出异常，就可以使用`not`匹配器。

  ```js
  const throwNewErrorFunc = ()=>{
      throw new Error('this is a new error')
  }
  
  test('toThrow匹配器',()=>{
      expect(throwNewErrorFunc).not.toThrow()
  })
  ```

- 现在这个测试用例就不能通过测试了，我们需要删除或注释掉抛出的异常，才可以通过测试。这就是`not`匹配器的用法。







## 四、让Jest支持import和ES6语法

- 目前Jest是不支持`import...from....`这种形式，如果使用就会报错，因为Jest默认支持的是`CommonJS`规范，也就是`Node.js`中的语法，他只支持`require`这种引用。所以我们使用`import...from...`是ES6的语法，所以使用就会报错。我们找到了报错的原因后，就非常好解决了，只要我们把`import`形式转行成`require`就可以了呗

- 先安装babel

  ```js
  pnpm install @babel/core@7.4.5 @babel/preset-env@7.4.5 -D
  ```

- Babel的基本配置

  - 我们在项目根目录下新建一个`.babelrc`的文件，作为一个前端，你对这个文件应该是非常熟悉的，babel的转换配置就写在这个文件里。

    ```js
    {
        "presets":[
            [
                    "@babel/preset-env",{
                    "targets":{
                        "node":"current"
                    }
                }
            ]
        ]
    }
    ```











## 五、异步代码测试方法

### 5.1 回调函数式

-  先安装axiso

  ```js
  pnpm install axios@0.19.0 --save
  ```

- 写代码进行测试

  - ```js
    // fetchData.js
    import axios from 'axios'
    
    export const fetchData = (fn)=>{
        axios.get('http://a.jspang.com/jestTest.json').then((response)=>{
            fn(response.data)
        })
    }
    ```

  - ```js
    // fetchData.spec.js
    面的代码。
    
    import { fetchData } from './fetchData.js'
    
    test('fetchData 测试',()=>{
       fetchData((data)=>{
           expect(data).toEqual({
               success: true
           })
    
       })
      })
    ```

    - 注意这样写是有问题的，因为**方法还没有等到回调，我们的结果已经完成了**，所以这时候你对于没测试完，只是方法可用，就返回了测试结果，**这种结果是不保证正确的**。

- **我们必须加入一个done方法，保证我们的回调已经完成了**，这时候我们表示测试完成。

  - ```js
    // fetchData.spec.js
    // done代表所有方法都完成了
    test("FetchData异步方法测试", (done) => {
        fetchData((data) => {
            // 测试返回数据是否正确
            expect(data).toEqual({
                success: true
            })
            // done();
        })
    })
    ```

  - 这时候我们的测试代码才能保证测试准确无误的完成，所以你在工作中处理异步函数的时候，一定要注意这种异步函数的形式如何来进行测试









### 5.2 直接返回promise

- 编写需要测试的代码

  ```js
  export const fetchTwoData = ()=>{
      return axios.get('http://a.jspang.com/jestTest.json')
  }
  ```

  - 从代码中可以看出，我们直接只用了`Return`返回了异步请求的`promise`值，这样的方法在实际工作中也经常使用

- 打开`fetchData.test.js`文件，然后新写一个测试用例，在写之前记得先把`fetchTwoData`方法引过来

  - 测试用例：

    ```js
    test("FetchTwoData直接返回promise测试", () => {
        // 这里要加return，否则jest不知道你的代码是异步的
        return fetchTwoData().then((response) => {
            expect(response.data).toEqual({
                success: true
            })
        })
    })
    ```

  - 这部分代码需要注意的是要**使用`return`才能测试成功**





### 5.3 接口不存在测试用例编写

-  在工作中有时候会测试异步接口不存在的需求（虽然不多，但这里有坑），比如有些后台需求不允许前台访问时，这时候就会返回404（页面不存在）

- 编写需要测试的代码

  - ```js
    export const fetchThreeData = ()=>{
        return axios.get('http://a.jspang.com/jestTest_error.json')
    }
    //注意这个地址是不存在的，也就是返回404
    ```

- 编写测试代码

  - ```js
    test("FetchThreeData错误测试", () => {
        return fetchTwoData().catch((e) => {
            // 有异常（检测到404）才通过测试
            expect(e.toString().indexOf('404') > -1).toBe(true);
        })
    })
    ```

  - 如果是这样子，无论地址存不存在都会通过

  - 因为只有出现异常的时候才会走catch，而没有出现异常的话，不会执行这个测试方法；所以jest就会默认这个用例通过了测试

- 要解决这个问题，需要用到`expect.assertions(1)`

  - ```js
    test("FetchThreeData错误测试", () => {
        expect.assertions(1); //断言，必须执行一次expect
        return fetchTwoData().catch((e) => {
            // 有异常（检测到404）才通过测试
            expect(e.toString().indexOf('404') > -1).toBe(true);
        })
    })
    ```

  - 这时候正确的地址就无法正常通过测试了，因为此时我们的地址是存在并正确返回结果的。

  - 我们需要改成错误的地址，才能通过测试。

    



### 5.4 async...await

- 编写需要测试的代码

  ```js
   export const fetchFourData = ()=>{
      return axios.get('http://a.jspang.com/jestTest.json')
  }
  ```

- 编写测试代码

  ```js
  test('FetchFourData 测试', async()=>{
          const response  = await fetchFourData()
          expect(response.data).toEqual({
              success : true
          })
  })
  ```







## 六、Jest中的四个钩子函数

- 程序与测试方法

  - ```js
    // newTest.js
    export default class NewTest {
        shangping(number) {
            this.user = number == 1 ? '可乐' : '雪碧'
        }
    
        jiezhang() {
            this.qian = this.user + '结账'
        }
        tuihuo() {
            this.qian = this.user + '退货'
        }
    }
    ```

  - ```js
    // newTest.spec.js
    import NewTest from "./newTest";
    
    const test1 = new NewTest()
    
    test('测试可乐结账', () => {
        test1.shangping(1);
        test1.jiezhang();
        console.log(test1.qian);
        expect(test1.qian).toEqual('可乐结账');
    })
    test('测试雪碧退货', () => {
        test1.shangping(2);
        test1.tuihuo();
        console.log(test1.qian);
        expect(test1.qian).toEqual('雪碧退货');
    })
    ```



### 6.1 beforeAll()钩子函数

- `beforeAll()`钩子函数的意思是在所有测试用例之前进行执行。

- 比如我们写一个这样的测试用例。

  ```js
  beforeAll(()=>{
      console.log('走进商场')
  })
  ```

- 写完后，保存文件，会自动执行测试。这时候可以在控制台看到`走进商场`,最新执行啦。执行之后可以看到这句话最先执行了。然后才走下面的测试用例。





### 6.2 afterAll()钩子函数

- `afterAll()`钩子函数是在完成所有测试用例之后才执行的函数。

- ```js
  afterAll(()=>{
      console.log('有钱人的生活就是这么的枯燥且寂寞')
  })
  ```

- 保存后，可以在控制台看到`afterAll`是最后执行的。这个用于测试都完成后调用某个方法。





### 6.3 beforeEach()钩子函数

- `beforeEach()`钩子函数，是在**每个测试用例前**都会执行一次的钩子函数，比如我们写如下代码。

  ```js
  beforeEach(()=>{
      console.log('给了300元钱后......')
  })
  ```

- 保存后可以看到，每个测试用例执行之前都会先执行一下这个函数





### 6.4 afterEach()钩子函数

- `afterEach()`钩子函数，是在每次测试用例完成测试之后执行一次的钩子函数，比如下面的代码。

  ```js
  afterEach(()=>{
      console.log('完成后，我心满意足的坐在沙发上！！！')
  })
  ```



- 全部测试代码

  ```js
  import NewTest from "./newTest";
  
  const test1 = new NewTest()
  
  beforeAll(()=>{
      console.log('走进商场')
  })
  
  beforeEach(()=>{
      console.log('给了300元钱后......')
  })
  
  test('测试可乐结账', () => {
      test1.shangping(1);
      test1.jiezhang();
      console.log(test1.qian);
      expect(test1.qian).toEqual('可乐结账');
  })
  test('测试雪碧退货', () => {
      test1.shangping(2);
      test1.tuihuo();
      console.log(test1.qian);
      expect(test1.qian).toEqual('雪碧退货');
  })
  
  afterAll(()=>{
      console.log('有钱人的生活就是这么的枯燥且寂寞')
  })
  
  afterEach(()=>{
      console.log('完成后，我心满意足的坐在沙发上！！！')
  })
  ```









## 七、Jest中对测试用例进行分组

- 接着上面的测试，添加了两个新方法

  - ```js
    export default class NewTest {
        shangping(number) {
            this.user = number == 1 ? '可乐' : '雪碧'
        }
    
        jiezhang() {
            this.qian = this.user + '结账'
        }
        tuihuo() {
            this.qian = this.user + '退货'
        }
    
        tiaoxuan() {
            this.qian = this.user + '挑选'
        }
        rengdiao() {
            this.qian = this.user + '扔掉'
        }
    }
    ```

  - ```js
    import NewTest from "./newTest";
    
    const test1 = new NewTest()
    
    beforeAll(() => {
        console.log('走进商场')
    })
    
    beforeEach(() => {
        console.log('给了300元钱后......')
    })
    
    test('测试可乐结账', () => {
        test1.shangping(1);
        test1.jiezhang();
        console.log(test1.qian);
        expect(test1.qian).toEqual('可乐结账');
    })
    test('测试雪碧退货', () => {
        test1.shangping(2);
        test1.tuihuo();
        console.log(test1.qian);
        expect(test1.qian).toEqual('雪碧退货');
    })
    
    test('测试可乐挑选', () => {
        test1.shangping(1);
        test1.tiaoxuan();
        console.log(test1.qian);
        expect(test1.qian).toEqual('可乐挑选');
    })
    test('测试雪碧扔掉', () => {
        test1.shangping(2);
        test1.rengdiao();
        console.log(test1.qian);
        expect(test1.qian).toEqual('雪碧扔掉');
    })
    
    afterAll(() => {
        console.log('有钱人的生活就是这么的枯燥且寂寞')
    })
    
    afterEach(() => {
        console.log('完成后，我心满意足的坐在沙发上！！！')
    })
    ```

    

- 随着项目的不断增多，我们是时候出个套餐服务了。所以就需要进行分组。

  - 那最原始的方法是，我们新建两个测试文件，把`可乐`的服务项放到一个测试文件里，把`雪碧`的测试文件放到一个文件里去，这就形成了分组
  - 但这样分组显然是不够优雅的，毕竟我们需要测试的代码在一个文件里，我们的测试文件却分成了两个文件。

- 其实`Jest`为我们提供了一个**分组的语法`describe()`,这个方法接受两个参数**，现在我们把可乐和雪碧的测试用例用`describe()`方法进行分开

  - ```js
    import NewTest from "./newTest";
    
    const test1 = new NewTest()
    
    beforeAll(() => {
        console.log('走进商场')
    })
    
    beforeEach(() => {
        console.log('给了300元钱后......')
    })
    
    
    describe("可乐", () => {
        test('测试可乐结账', () => {
            test1.shangping(1);
            test1.jiezhang();
            console.log(test1.qian);
            expect(test1.qian).toEqual('可乐结账');
        })
    
        test('测试可乐挑选', () => {
            test1.shangping(1);
            test1.tiaoxuan();
            console.log(test1.qian);
            expect(test1.qian).toEqual('可乐挑选');
        })
    })
    
    
    describe("雪碧", () => {
        test('测试雪碧退货', () => {
            test1.shangping(2);
            test1.tuihuo();
            console.log(test1.qian);
            expect(test1.qian).toEqual('雪碧退货');
        })
    
    
        test('测试雪碧扔掉', () => {
            test1.shangping(2);
            test1.rengdiao();
            console.log(test1.qian);
            expect(test1.qian).toEqual('雪碧扔掉');
        })
    })
    
    
    afterAll(() => {
        console.log('有钱人的生活就是这么的枯燥且寂寞')
    })
    
    afterEach(() => {
        console.log('完成后，我心满意足的坐在沙发上！！！')
    })
    ```

  - 这时候我们结束一下测试，然后清一下屏幕，再进行测试，这时候你可以清楚看到代码是分组进行测试的。这样分组的好处实际就就是要让测试用例开起来更有层次感

    









## 八、钩子函数的作用域

- 钩子函数的作用域有三个特点：
  - 钩子函数在父级分组可作用域子集，类似继承
  - 钩子函数同级分组作用域互不干扰，各起作用
  - 先执行外部的钩子函数，再执行内部的钩子函数



### 8.1 钩子函数在父级分组可作用域子集

- 在程序最外层加一个`describe`

  ```js
  import NewTest from "./newTest";
  
  const test1 = new NewTest()
  
  describe('最外层分组', () => {
      beforeAll(() => {
          console.log('走进商场')
      })
  
      beforeEach(() => {
          console.log('给了300元钱后......')
      })
  
  
      describe("可乐", () => {
          test('测试可乐结账', () => {
              test1.shangping(1);
              test1.jiezhang();
              console.log(test1.qian);
              expect(test1.qian).toEqual('可乐结账');
          })
  
          test('测试可乐挑选', () => {
              test1.shangping(1);
              test1.tiaoxuan();
              console.log(test1.qian);
              expect(test1.qian).toEqual('可乐挑选');
          })
      })
  
  
      describe("雪碧", () => {
          test('测试雪碧退货', () => {
              test1.shangping(2);
              test1.tuihuo();
              console.log(test1.qian);
              expect(test1.qian).toEqual('雪碧退货');
          })
  
  
          test('测试雪碧扔掉', () => {
              test1.shangping(2);
              test1.rengdiao();
              console.log(test1.qian);
              expect(test1.qian).toEqual('雪碧扔掉');
          })
      })
  
  
      afterAll(() => {
          console.log('有钱人的生活就是这么的枯燥且寂寞')
      })
  
      afterEach(() => {
          console.log('完成后，我心满意足的坐在沙发上！！！')
      })
  })
  ```

- 写完后在控制台运行`pnpm test`，可以看到`console.log`的顺序和结果并没有改变。并且每一个`beforeEach`和`afterEach`也都在每一个测试用例的前后执行了。这就是我们说的第一条**钩子函数在父级分组可作用域子集，类似继承**





### 8.2 钩子函数同级分组互不干扰 和 **先执行外部的钩子函数**

- 现在雪碧和可乐都要交税，但是税不同。这时候就可以在两个统计的`describe`中分别加入不同的`afterEach`，比如雪碧要3元税，可乐要5元税

- ```js
  import NewTest from "./newTest";
  
  const test1 = new NewTest()
  
  describe('最外层分组', () => {
      beforeAll(() => {
          console.log('走进商场')
      })
  
      beforeEach(() => {
          console.log('给了300元钱后......')
      })
  
  
      describe("可乐", () => {
          test('测试可乐结账', () => {
              test1.shangping(1);
              test1.jiezhang();
              console.log(test1.qian);
              expect(test1.qian).toEqual('可乐结账');
          })
  
          test('测试可乐挑选', () => {
              test1.shangping(1);
              test1.tiaoxuan();
              console.log(test1.qian);
              expect(test1.qian).toEqual('可乐挑选');
          })
  
          afterEach(() => {
              console.log('可乐5元税')
          })
      })
  
  
      describe("雪碧", () => {
          test('测试雪碧退货', () => {
              test1.shangping(2);
              test1.tuihuo();
              console.log(test1.qian);
              expect(test1.qian).toEqual('雪碧退货');
          })
  
  
          test('测试雪碧扔掉', () => {
              test1.shangping(2);
              test1.rengdiao();
              console.log(test1.qian);
              expect(test1.qian).toEqual('雪碧扔掉');
          })
  
          afterEach(() => {
              console.log('雪碧3元税')
          })
      })
  
  
      afterAll(() => {
          console.log('有钱人的生活就是这么的枯燥且寂寞')
      })
  
  
  })
  ```

- 结果：

  ```js
  	console.log newTest.spec.js:7
        走进商场
      console.log newTest.spec.js:11
        给了300元钱后......
      console.log newTest.spec.js:19
        可乐结账
      console.log newTest.spec.js:31
        可乐5元税
      console.log newTest.spec.js:11
        给了300元钱后......
      console.log newTest.spec.js:26
        可乐挑选
      console.log newTest.spec.js:31
        可乐5元税
      console.log newTest.spec.js:11
        给了300元钱后......
      console.log newTest.spec.js:40
        雪碧退货
      console.log newTest.spec.js:53
        雪碧3元税
      console.log newTest.spec.js:11
        给了300元钱后......
      console.log newTest.spec.js:48
        雪碧扔掉
      console.log newTest.spec.js:53
        雪碧3元税
      console.log newTest.spec.js:59
        有钱人的生活就是这么的枯燥且寂寞
  ```

- 这个案例也说明了钩子函数在同级的`describe`分组里是互不干扰的。

- 也可以看出外部的钩子比内部的先执行









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
- 
- 