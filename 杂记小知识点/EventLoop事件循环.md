# EventLoop事件循环



## EventLoop基本概念



### 同步任务

同步任务的执行：依次将同步任务调入主线程中，当前的同步任务执行完毕后，再调入下一个同步任务。

用一个栈来表示主线程：

![image-20220715162012310](EventLoop事件循环.assets/image-20220715162012310.png)



### 异步任务

异步任务会在同步任务执行之后再去执行，那么如果异步任务代码在同步任务代码之前呢？在js机制里，存在一个队列，叫做任务队列，专门用来存放异步任务。也就是说，**当异步任务出现的时候，会先将异步任务存放在任务队列中，当执行完所有的同步任务之后，再去调用任务队列中的异步任务**

<img src="EventLoop事件循环.assets/image-20220715161902509.png" alt="image-20220715161902509" style="zoom: 67%;" />

js会先将同步任务1提至主线程，然后发现异步任务1和2，则将异步任务1和2依次放入任务队列；

然后同步任务1执行完之后出栈，又将同步任务2入栈执行

当同步任务2执行完之后，再从任务队列中提取异步任务执行

<img src="EventLoop事件循环.assets/image-20220715161946336.png" alt="image-20220715161946336" style="zoom:67%;" />



### 有哪些宏任务与微任务

异步任务分为宏任务与微任务，所以上述的任务队列，也分为宏任务队列以及微任务队列。

> 宏任务：I/O、定时器、事件绑定、ajax等
>
> 微任务：Promise的then、catch、finally和process的nextTick，await后面的语句

而在js中，微任务总是优先于宏任务，所以 ：

 同步任务 > 异步任务  =>   同步任务 > 微任务 > 宏任务

### Promise

#### Promise内的代码

```js
console.log(1);
setTimeout(function(){
    console.log(2)
},0)
new Promise((resolve,reject)=>{
    console.log(3)
    resolve()
    console.log(4)
}).then(()=>{
    console.log(5)
})
console.log(6)

/**
最后执行结果：
1
3
4
6
5
2

有一个要注意的点：Promise的then方法是微任务，但是Promise里面的代码是同步任务
*/
```



#### Promise.then方法

```js
console.log("start");
setTimeout(() => {
    console.log("children2")
    Promise.resolve()
        .then(() => {
            console.log("children3")
        })
}, 0)
new Promise(function (resolve, reject) {
    console.log("children4")
    setTimeout(function () {
        console.log("children5")
        resolve("children6")
    }, 0)
}).then(res => {
    console.log("children7")
    setTimeout(() => { console.log(res) }, 0)
})

/**
输出结果：
start
children4
children2
children3
children5
children7
children6

注意点：Promise的then虽然是微任务，但是异步函数在相应辅助线程中处理完成后，即异步函数达到触发条件了，就把回调函数推入任务队列中，而不是说注册一个异步任务就会被放在这个任务队列中。

也就是说：then方法推入微任务队列的前提是 执行了 resolve 或者是 reject 方法，这时候才能够将注册的回调函数推入微任务队列。
*/
```





### async await函数

await语句可以看作同步代码，而await后面的代码，需要看作微任务

```js
async function async1() {
    console.log("async1 start")
    await async2()
    console.log("async1 end")
}
async function async2() {
    console.log("async2")
}
console.log("script start")
setTimeout(function () {
    console.log("setTimeout")
}, 0)
async1()
new Promise(function (resolve) {
    console.log("promise1")
    resolve()
}).then(function () {
    console.log("promise2")
})
console.log("script end")

/**
输出结果：
script start
async1 start
async2
promise1
script end
async1 end
promise2
setTimeout
*/
```



### nextTick函数

当promise的then()和process的nextTick()处在同一个宏任务中时，nextTick()先于then()执行。

```js
new Promise(function (resolve) {
    console.log(3);
    resolve();
    console.log(4);
}).then(function () {
    console.log(5);
});
process.nextTick(function () {
    console.log(8);
});
process.nextTick(function () {
    console.log(7);
});

/**
最后结果：
3
4
8
7
5
*/
```



nextTick函数也是一种微任务，但是它是一种特殊的微任务：

> process.nextTick()是Node环境下的方法， 所以我们基于Node谈论。
>
> process.nextTick()是一个特殊的异步API，其不属于任何的Event Loop阶段。事实上Node在遇到这个API时，Event Loop根本就不会继续进行，会马上停下来执行process.nextTick()，这个执行完后才会继续Event Loop。





### 怎样才算一个循环呢

是一次宏任务加上当前所有微任务执行。

当js执行时，先执行宏任务（其实，就是当前宏任务中所有的同步任务），遇到微任务，则推入微队列，并注册回调函数。

当该次宏任务执行完后，会读取微任务注册的回调函数到执行栈中执行。执行完后。继续下一轮循环。





## 深入





### 为什么会有EventLoop

我们的js是单线程的，这也导致了很对的任务需要排队，执行完了一个任务之后才能执行下一个任务，当一个任务需要执行很长的时间的时候，就容易造成阻塞。

于是就引入了事件循环的机制。

事件循环机制将任务分为同步任务以及异步任务。

> 同步任务：立即执行的任务，在主线程上排队执行，前一个任务执行完毕，才能执行下一个任务。
>
> 异步任务：异步执行的任务，不进去主线程，而是在异步任务有了结果之后，将注册的回调函数放入任务队列中等待主线程空闲的时候读取执行。
>
> 注意：异步函数是在相应的辅助线程中处理完成后，即异步函数达到触发条件了，就把回调函数推入任务队列中，而不是注册了一个异步任务就会把回调函数放在任务队列中。



### 为什么js是单线程的

多线程的效率更高，但是假设一种情况：两个线程同时对一个dom元素进行修改，那么这时候浏览器应该怎么办？

所以，js只能是单线程的。

但是要区分，js是单线程不代表浏览器是单线程，实际上，浏览器是多线程的：

- js执行线程
- GUI渲染线程
- 事件监听线程
- 计时器计时线程
- 网络通信线程





### 为什么要有微任务，不能只有宏任务吗

> 就是我们去银行办理业务时， 并不是到了就能办理， 而是需要先取号排队， 等到柜台业务员办理完当前客户业务才能继续叫号进行下一个。
>
> 这时每一个来办理业务的人就可以认为是银行柜员的一个宏任务来存在， 当办理到你的业务时， 你本来只是要重新绑定一下手机号， 但是突然想到明天要参加婚礼，需要随份子钱， 此时你和柜员说你要取money, 这时候柜员不能告诉你，让你重新取号排队（不合理的要求）。
>
> 其实这时候就相当于你突然提出了一个新的任务，这个任务就相当于是一个微任务 ,它要在下一个宏任务之前完成。

因为当微任务没有执行完毕时，是不会执行下一个宏任务的。

宏任务只能按照先进先出的原则执行，一些高优先级的任务就无法先执行。还需要执行优先级更高的。于是还要有微任务。



### setTimeOut的不准确性

很多时候都会那setTimeout拿来做异步的宏任务的例子，但是思考一件事，setTimeout是什么时候开始计时的呢？

是在检测到setTimeout并且将setTimeout放入宏任务队列的时候开始计时，还是在执行完同步代码以及微任务队列后，读取下一个宏任务的时候开始计时呢？

> MDN官方文档：
>
> #### 超时延迟
>
> 除了"最小延时"之外，定时器仍然有可能因为当前页面（或者操作系统/浏览器本身）被其他任务占用导致延时。 需要被强调是， 直到调用 `setTimeout()`的主线程执行完其他任务之后，回调函数和代码段才能被执行。例如：
>
> 你不知道的javascript：
>
> setTimeout并没有把你的回调函数挂在时间循环队列中。他所做的是设定一个定时器，当定时器到时后，环境会把你的回调函数放在事件循环中。

