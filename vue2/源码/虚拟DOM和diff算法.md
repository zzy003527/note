## 虚拟DOM和diff算法

- snabbdom是什么

  - snabbdom是著名的虚拟DOM库，是diff算法的鼻祖，Vue源码借鉴了snabbdom

- 安装snabbdom

  - 在git上的snabbdom源码是用ts写的，git上并不提供编译好的js版本
  - 如果要使用js版的snabbdom库，可以从npm上下载：`npm i -S snabbdom`

- snabbdom的测试环境搭建

  - snabbdom库是DOM库，不能在nodejs环境运行，所以我们需要搭建webpack和webpack-dev-server开发环境
  - 这里要注意，必须按照最新版webpack@5，因为webpack4没有读取身份证中exports的能力
  - `npm i -D webpack@5 webpack-cli@3 webpack-dev-server@3`

- 跑通官方git首页的demo程序

  - 跑通snabbdom官方git首页的demo程序，即证明调试环境已经搭建成功

    - https://github.com/snabbdom/snabbdom

  - 不要忘记在index.html中放置一个div#container

    



## 虚拟DOM

- #### 什么是虚拟DOM

  - 用JavaScript对象描述DOM的层次结构。DOM中的一切属性都在虚拟DOM中有对应的属性
  - diff是发生在虚拟DOM上的
    - 新虚拟DOM和老虚拟DOM进行diff（精细化比较），算出应该如何最小量更新，最后反映到真正的DOM上0

- #### h函数

  - h函数用来产生**虚拟节点（vnode）**

  - 比如这样调用h函数：

    - ```js
      h('a',{ props: {href: 'http://www.baidu.com'}},'百度');
      ```

    - 将得到这样的虚拟节点：

      ```js
      {"sel":"a","data": { props: { href: 'http://www.baidu.com'}},"text":"百度"}
      ```

    -  他表示的真正的DOM节点：

      ```html
      <a href="http://www.baidu.com">百度</a>
      ```

  - 一个虚拟节点有哪些属性？

    - ```js
      {
          children: undefined        //它的子元素
          data: {}               //身上的属性、样式
          elm: undefined            //这个虚拟节点对应的真实DOM节点
          key: undefined          //这个节点的唯一表示
          sel: "div"              //选择器（selector）
          text: "我是一个盒子"          //文字
      }
      ```

  - 创建一个节点并渲染

    - ```js
      // 创建出patch函数
      const patch = init([classModule, propsModule, styleModule, eventListenersModule]);
      
      // 创建虚拟节点
      const myVnode1 = h('a', {
          props: {
              href: 'http://www.baidu.com',
              target: '_blank'
          }
      }, '百度')
      
      // 让虚拟节点上树
      const container = document.getElementById("container");
      // 第一个参数放container，第二个参数放虚拟节点
      patch(container, myVnode1)
      ```

  - **h函数可以嵌套使用，从而得到虚拟DOM树**

    - 比如这样嵌套使用h函数：

      ```js
      h('ul',{},[
          h('li',{},'牛奶'),
          h('li',{},'咖啡'),
          h('li',{},'可乐')
      ])
      ```

    - 将得到这样的虚拟DOM树：

      ```js
      {
          "sel": "ul",
          "data": {},
          "children": [
              { "sel":"li","text":"牛奶"},
              { "sel":"li","text":"咖啡"},
              { "sel":"li","text":"可乐"}
          ]
      }
      ```

    

  - #### 手写h函数

    - 首先要写一个vnode函数

      - 这个函数的功能就是把传入的函数的参数组合成对象返回

      ```js
      export default function (sel, data, children, text, elm) {
          const key = data === undefined ? undefined : data.key;
          return {
              sel,
              data,
              children,
              text,
              elm,
              key
          }
      }
      ```

    -  然后写h函数

      - ```js
        import vnode from './vnode.js'
        
        // 一个低配版本的h函数，这个函数必须接受3个参数。缺一不可
        // 相当于它的重载功能较弱
        // 也就是说，调用的形态必须是下列三种之一：
        /*
        形态一：h('div',{},'文字')
        形态二：h('div',{},[])
        形态三：h('div',{},h())
        */
        export default function (sel, data, c) {
            // 检查参数的个数
            if (arguments.length != 3) {
                throw new Error('对不起，我写的h函数必须传入3个参数')
            }
            // 检查c的类型
            if (typeof c == 'string' || typeof c == 'number') {
                // 说明是形态一，返回一个vnode
                return vnode(sel, data, undefined, c, undefined);
            } else if (Array.isArray(c)) {
                // 说明是形态二
                let children = []
                // 遍历c,收集子元素
                for (let i = 0; i < c.length; i++) {
                    // 检查c[i]必须是一个对象
                    if (!(typeof c[i] == 'object' && c[i].hasOwnProperty('sel'))) {
                        throw new Error('传入的数组参数中有项不是h函数')
                    }
                    // 此时需要收集子节点
                    children.push(c[i])
                }
                // 循环结束了，就说明children收集完毕了，可以返回带有children属性的vnode了
                return vnode(sel, data, children, undefined, undefined)
            } else if (typeof c == 'object' && c.hasOwnProperty('sel')) {
                // 如果为对象且c有sel属性（调用h函数返回的vnode一定有sel），那就说明是形态三
                // 即传入的c为唯一的children
                let children = [c]
                return vnode(sel, data, children, undefined, undefined)
            } else {
                throw new Error('传入的第三个参数类型不对')
            }
        }
        ```

  - 

  - 

  - d

- #### diff算法

  - ##### diff算法注意点

    - **只有同一个虚拟节点，才会进行精细化比较**

      - 否则就算暴力删除旧的节点、插入新的节点
      - **延申：如何定义是同一个虚拟节点？**
        - **选择器相同且key相同**

    - **只进行同层比较，不会进行跨层比较**

      - 即使是同一片虚拟节点，但是跨层了，那也不会精细化比较。只会暴力删除旧节点、插入新结点

      - 比如：

        ```js
        const vnode1 = h('div',{},[
            h('p',{key:'A'},'A'),
            h('p',{key:'B'},'B'),
            h('p',{key:'C'},'C'),
            h('p',{key:'D'},'D')
        ])
        
        const vnode2 = h('div',{},h('section',{},[
            h('p',{key:'A'},'A'),
            h('p',{key:'B'},'B'),
            h('p',{key:'C'},'C'),
            h('p',{key:'D'},'D')
        ]))
        
        //这样子vnode2比vnode1就多了一层
        ```

    - **key很重要**

      - **key是这个节点的唯一表示，告诉diff算法，在更改前后它们是同一个DOM节点**

      

  - diff算法实现思路

    - ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210415222737884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70#pic_center)

    - 当patch函数被调用时：`patch(oldVnode,newVnode)`

      - <u>判断oldVnode是虚拟节点还是DOM节点</u>
      - 如果是DOM节点，那将其包装为虚拟节点，再进行下一步
      - 如果是虚拟节点，直接下一步

    - <u>判断oldVnode和newVnode的key和sel（选择器）是否相同</u>

      - 如果不是，那么暴力删除旧节点，插入新节点

      - 如果是，进行**精细化比较**

      - 判断key和sel（选择器）是否相同

        ```js
        function sameVnode(vnode1,vnode2) {
            return vnode1.key === vnode2.key && vnode1.sel === vnode2.sel
        }
        ```

    - 如何精细化比较

      - <u>判断oldVnode和newVnode是否为内存中的同一个对象</u>
        - 如果是，那就什么都不用做了
        - 如果不是，进入下一步
        
      - <u>判断newVnode有没有text属性</u>
        - 如果有，判断：
          - <u>newVnode的text和oldVnode的是否相同</u>
            - 若相同，什么都不用做
            - 若不同，**把elm中的innerText改变为新的text（即使oldVnode有children属性而没有text属性也没事，因为innerText一旦改变为新的text，老节点的children也会没了）**
        - 如果没有（意味着newVnode有children），那么<u>判断oldVnode有没有子节点</u>
          - 如果没有（意味着oldVnode有text），那么需要**清空oldVnode中的text**，并且**把newVnode的children添加到DOM中**
          - **<u>如果有（新老节点都有children）</u>**，此时要进行**最难的diff**
        
      - #### diff算法的子节点更新策略

        - 四种命中查找

          - 新前与旧前

          - 新后与旧后

          - 新后与旧前

            - 命中时，此时要移动节点。**移动<u>新后</u>指向的这个节点<u>到旧后的后面</u>**

          - 新前与旧后

            - 命中时，此时要移动节点。**移动<u>新前</u>指向的这个节点<u>到旧前的前面</u>**

          - **注意：**

            - **按顺序向下查找**

            - **命中一种就不再按顺序进行命中判断，下移后重新判断命中**

            - **如果都没命中，就需要用循环来寻找<u>新前</u>，之后在虚拟DOM中将其设为undefined，并在真实DOM中将其移动到<u>旧前(oldSrartIdx)之前</u>**

            - **判断条件**

              ```js
              while(新前<=新后 && 旧前<=旧后) {
                  //如果旧节点先循环完毕，而新结点中还有剩余节点，那么说明新结点中有要插入的节点
                  //如果新节点先循环完毕，而老结点中还有剩余节点（旧前和旧后之间的节点），那么说明老节点中有要删除的节点   
              }
              ```

            - 

          - 例子：

            ```js
            //旧子节点
            h('li',{ key: 'A' }, 'A')            //旧前
            h('li',{ key: 'B' }, 'B')
            h('li',{ key: 'C' }, 'C')            //旧后
            
            //新子节点
            h('li',{ key: 'A' }, 'A')            //新前
            h('li',{ key: 'B' }, 'B')
            h('li',{ key: 'C' }, 'C')
            h('li',{ key: 'D' }, 'D')
            h('li',{ key: 'E' }, 'E')            //新后
            ```

        - 此时patchVNode修改为：

          ```js
          //patchVNode.js
          import createElement from "./createElement.js";
          import updateChildren from "./updateChildren.js";
          
          // diff精细化比较,对比同一个虚拟节点
          export default function patchVNode(oldVnode, newVnode) {
              // 判断新旧vnode是否为同一个对象
              if (oldVnode === newVnode) {
                  return;
              }
              // 判断新vnode有没有text属性
              if (newVnode.text != undefined && newVnode.children == undefined || newVnode.children.length == 0) {
                  // 判断text是否与之前相同
                  if (newVnode.text != oldVnode.text) {
                      // 将老节点的text更新
                      oldVnode.elm.innerText = newVnode.text;
                  }
              } else {
                  // 新vnode没有text属性
                  // 判断oldVnode有没有children属性
                  if (oldVnode.children != undefined && oldVnode.children.length > 0) {
                      // 老的有children，此时是最复杂的情况，就是新老都有children
                      //--------------------------------此处引用--------------------------
                      updateChildren(oldVnode.elm, oldVnode.children, newVnode.children)
                      //--------------------------------此处引用--------------------------
                  } else {
                      //老的没有children，新的有children
                      // 清空oldVnode的节点中的text
                      oldVnode.elm.innerHTML = ''
                      // 将newVnode中的元素添加到oldVnode中
                      for (let i = 0; i < newVnode.children.length; i++) {
                          let dom = createElement(newVnode.children[i])
                          oldVnode.elm.appendChild(dom)
                      }
          
                  }
          
              }
          }
          ```

        - updateChildren为：

          ```js
          import createElement from "./createElement.js";
          import patchVnode from "./patchVNode.js";
          /**
           * 
           * @param {object} parentElm Dom节点
           * @param {Array} oldCh oldVnode的子节点数组
           * @param {Array} newCh newVnode的子节点数组
           */
          export default function updateChildren(parentElm, oldCh, newCh) {
              console.log("updateChildren()");
              console.log(oldCh, newCh);
          
              // 四个指针
              // 旧前
              let oldStartIdx = 0;
              // 新前
              let newStartIdx = 0;
              // 旧后
              let oldEndIdx = oldCh.length - 1;
              // 新后
              let newEndIdx = newCh.length - 1;
          
              // 指针指向的四个节点
              // 旧前节点
              let oldStartVnode = oldCh[0];
              // 旧后节点
              let oldEndVnode = oldCh[oldEndIdx];
              // 新前节点
              let newStartVnode = newCh[0];
              // 新后节点
              let newEndVnode = newCh[newEndIdx];
          
              let keyMap = null;
          
              while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
                  console.log("**循环中**");
                  // 首先应该不是判断四种命中，而是略过已经加了undefined标记的项
                  if (oldStartVnode === null || oldCh[oldStartIdx] === undefined) {
                      oldStartVnode = oldCh[++oldStartIdx];
                  } else if (oldEndVnode === null || oldCh[oldEndIdx] === undefined) {
                      oldEndVnode = oldCh[--oldEndIdx];
                  } else if (newStartVnode === null || newCh[newStartIdx] === undefined) {
                      newStartVnode = newCh[++newStartIdx];
                  } else if (newEndVnode === null || newCh[newEndIdx] === undefined) {
                      newEndVnode = newCh[--newEndIdx];
                  } else if (checkSameVnode(oldStartVnode, newStartVnode)) {
                      // 新前与旧前
                      console.log(" ①1 新前与旧前 命中");
                      // 精细化比较两个节点 oldStartVnode现在和newStartVnode一样了
                      patchVnode(oldStartVnode, newStartVnode);
                      // 移动指针，改变指针指向的节点，这表示这两个节点都处理（比较）完了
                      oldStartVnode = oldCh[++oldStartIdx];
                      newStartVnode = newCh[++newStartIdx];
                  } else if (checkSameVnode(oldEndVnode, newEndVnode)) {
                      // 新后与旧后
                      console.log(" ②2 新后与旧后 命中");
                      patchVnode(oldEndVnode, newEndVnode);
                      oldEndVnode = oldCh[--oldEndIdx];
                      newEndVnode = newCh[--newEndIdx];
                  } else if (checkSameVnode(oldStartVnode, newEndVnode)) {
                      // 新后与旧前
                      console.log(" ③3 新后与旧前 命中");
                      patchVnode(oldStartVnode, newEndVnode);
                      // 当③新后与旧前命中的时候，此时要移动节点。移动 新后（旧前） 指向的这个节点到老节点的 旧后的后面
                      // 移动节点：只要插入一个已经在DOM树上 的节点，就会被移动
                      parentElm.insertBefore(oldStartVnode.elm, oldEndVnode.elm.nextSibling);
                      oldStartVnode = oldCh[++oldStartIdx];
                      newEndVnode = newCh[--newEndIdx];
                  } else if (checkSameVnode(oldEndVnode, newStartVnode)) {
                      // 新前与旧后
                      console.log(" ④4 新前与旧后 命中");
                      patchVnode(oldEndVnode, newStartVnode);
                      // 当④新前与旧后命中的时候，此时要移动节点。移动 新前（旧后） 指向的这个节点到老节点的 旧前的前面
                      // 移动节点：只要插入一个已经在DOM树上的节点，就会被移动
                      parentElm.insertBefore(oldEndVnode.elm, oldStartVnode.elm);
                      oldEndVnode = oldCh[--oldEndIdx];
                      newStartVnode = newCh[++newStartIdx];
                  } else {
                      // 四种都没有匹配到，都没有命中
                      console.log("四种都没有命中");
                      // 寻找 keyMap 一个映射对象， 就不用每次都遍历old对象了
                      if (!keyMap) {
                          keyMap = {};
                          // 记录oldVnode中的节点出现的key
                          // 从oldStartIdx开始到oldEndIdx结束，创建keyMap
                          for (let i = oldStartIdx; i <= oldEndIdx; i++) {
                              const key = oldCh[i].key;
                              if (key !== undefined) {
                                  keyMap[key] = i;
                              }
                          }
                      }
                      console.log(keyMap);
                      // 寻找当前项（newStartIdx）在keyMap中映射的序号
                      const idxInOld = keyMap[newStartVnode.key];
                      if (idxInOld === undefined) {
                          // 如果 idxInOld 是 undefined 说明是全新的项，要插入
                          // 被加入的项（就是newStartVnode这项)现不是真正的DOM节点
                          parentElm.insertBefore(createElement(newStartVnode), oldStartVnode.elm);
                      } else {
                          // 说明不是全新的项，要移动
                          const elmToMove = oldCh[idxInOld];
                          patchVnode(elmToMove, newStartVnode);
                          // 把这项设置为undefined，表示我已经处理完这项了
                          oldCh[idxInOld] = undefined;
                          // 移动，调用insertBefore也可以实现移动。
                          parentElm.insertBefore(elmToMove.elm, oldStartVnode.elm);
                      }
          
                      // newStartIdx++;
                      newStartVnode = newCh[++newStartIdx];
                  }
              }
              // 循环结束
              if (newStartIdx <= newEndIdx) {
                  // 说明newVndoe还有剩余节点没有处理，所以要添加这些节点
                  for (let i = newStartIdx; i <= newEndIdx; i++) {
                      // insertBefore方法可以自动识别null，如果是null就会自动排到队尾，和appendChild一致
                      parentElm.insertBefore(createElement(newCh[i]), oldCh[oldStartIdx].elm);
                  }
              } else if (oldStartIdx <= oldEndIdx) {
                  // 说明oldVnode还有剩余节点没有处理，所以要删除这些节点
                  for (let i = oldStartIdx; i <= oldEndIdx; i++) {
                      if (oldCh[i]) {
                          parentElm.removeChild(oldCh[i].elm);
                      }
                  }
              }
          }
          
          // 判断是否是同一个节点
          function checkSameVnode(a, b) {
              return a.sel === b.sel && a.key === b.key;
          }
          ```

          

  - 

- 完，整不懂了   : -(
