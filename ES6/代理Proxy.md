## 代理Proxy

- 代理的handler相当于对target设置了一层拦截，每次通过代理p访问target，都需要先进入handler进行判断和返回，如果handler为空，才会直接访问target

- 语法：

  - ```js
     let p = new Proxy(target, handler);
    ```

  - **target（要代理谁）**：一个目标对象(可以是任何类型的对象，包括本机数组，函数，甚至另一个代理)用Proxy来包装。

  - **handler（要做什么事情）**：一个对象，其属性是当执行一个操作时定义代理的行为的函数。

- 代理使用

  - **基础demo：**Proxy的demo有很多，我们只分析基础demo，主要看new Proxy({}, handler)的操作，**指定目标obj对象，然后handler对象执行get()操作**，get()返回值的判断是，如**果name是target目标对象的属性，则返回target[name]的值，否则返回37**，最后测试的时候，p.a是对象p的key，所以返回a的value，而p.b不存在，返回37。

  - ```js
    const obj = {
          a: 10
        }
    
    let handler = {
            get: function(target, name){
                console.log('test: ', target, name)  //get中的target是代理函数中的obj，name是obj中的属性名
                // test:  {"a":10} a
                // test:  {"a":10} b
                return name in target ? target[name] : 37
            },
            set: function(target,attr,value) {      //value是要给target[attr]设置的值
                if(value>=0&&value<150) {                //此处相当于设置拦截
                   target[attr] = value;
                } else {
                    console.warn("不能设置超出年龄范围的数字");
                }
            }
        }
    
    
        let p = new Proxy(obj, handler)
        console.log(p.a, p.b) // 10 37
        //p是代理函数，p.a指p中第一个参数obj的属性名a，将a传入第二个参数的name中，进行判断和操作
    
        p.a = 130;       //这样调用了set函数，为obj的a属性设置了值130(通过p对obj进行了修改，这就是代理)
        p.a = 200;         //会在set中生成警告 
    
    //这个例子的作用是拦截目标对象obj，当执行obj的读写操作时，进入handler函数进行判断，如果读取的key不存在，则返回默认值。
    ```

  - d