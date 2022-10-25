# Vue初始化过程

## 一.Vue初始化过程都做了什么

- Vue 的初始化过程（new Vue(options)）都做了什么？

  1. 处理组件配置项(用户选项和系统默认的选项进行合并)

  2. - 初始化根组件时进行了选项合并操作，将全局配置合并到根组件的局部配置上
     - 初始化每个子组件时做了一些性能优化，将组件配置对象上的一些深层次属性放到 vm.$options 选项中，以提高代码的执行效率

  3. 初始化组件实例的关系属性，比如 parent、children、root、refs 等

  4. 处理自定义事件

  5. 初始化render渲染所需的slots、渲染函数等。

     - 其实就两件事：插槽的处理 和 $createElm的声明，也就是 render 函数中的 h 的声明（h函数就是用来产生虚拟节点的）

  6. 调用 beforeCreate 钩子函数

  7. 初始化组件的 inject 配置项，得到 `ret[key] = val` 形式的配置对象，然后对该配置对象进行浅层的响应式处理（只处理了对象第一层数据），并代理每个 key 到 vm 实例上

     - 作为一个组件，在要给后辈组件提供数据之前，需要先把祖辈传下来的数据注入进来

  8. 数据响应式，处理 props、methods、data、computed、watch 等选项

  9. 解析组件配置项上的 provide 对象，将其挂载到 vm._provided 属性上

  10. 调用 created 钩子函数，初始化完成，可以进行挂载了

  11. 如果发现配置项上有 el 选项，则自动调用 方法，也就是说有了选项，就不需要再手动调用mount 方法，反之，没提供 el 选项则必须调用 $mount

  12. 接下来则进入挂载阶段





## 二.源码

### 1.index.js

- ```js
  //   /src/core/instance/index.js
  
  import { initMixin } from './init'
  
  // Vue 构造函数
  function Vue (options) {
    // 调用 Vue.prototype._init 方法，该方法是在 initMixin 中定义的
    this._init(options)
  }
  
  // 定义 Vue.prototype._init 方法
  initMixin(Vue)
  
  export default Vue
  ```

- 

- d



### 2.处理组件配置项(/src/core/instance/init.js)

- ```js
  //     /src/core/instance/init.js
  
  /**
   * 定义 Vue.prototype._init 方法 
   * @param {*} Vue Vue 构造函数
   */
  export function initMixin (Vue: Class<Component>) {
    // 负责 Vue 的初始化过程
    Vue.prototype._init = function (options?: Object) {
      // vue 实例
      const vm: Component = this
      // 每个 vue 实例都有一个 _uid，并且是依次递增的
      vm._uid = uid++
  
      // a flag to avoid this being observed
      vm._isVue = true
      // 处理组件配置项(上方事件1)
      if (options && options._isComponent) {
        /**
         * 每个子组件初始化时走这里，这里只做了一些性能优化
         * 将组件配置对象上的一些深层次属性放到 vm.$options 选项中，以提高代码的执行效率
         */
        // 2.1
        initInternalComponent(vm, options)
      } else {
        // 只有根组件走这里
        /**
         * 初始化根组件时走这里，合并 Vue 的全局配置到根组件的局部配置，比如 Vue.component 注册的全局组件会合并到 根实例的 components 选项中
         * 至于每个子组件的选项合并则发生在两个地方：
         *   1、Vue.component 方法注册的全局组件在注册时做了选项合并
         *   2、{ components: { xx } } 方式注册的局部组件在执行编译器生成的 render 函数时做了选项合并，包括根组件中的 components 配置
         */
        // 2.3
        vm.$options = mergeOptions(
          // 2.2
          resolveConstructorOptions(vm.constructor),
          options || {},
          vm
        )
      }
        
      // 这个是不重要的
      /* istanbul ignore else */
      if (process.env.NODE_ENV !== 'production') {
        // 设置代理，将 vm 实例上的属性代理到 vm._renderProxy
        initProxy(vm)
      } else {
        vm._renderProxy = vm
      }
        
        
        
      // expose real self
      vm._self = vm
        
      //重点，整个初始化最重要的部分  
        
      // 初始化组件实例关系属性，比如 $parent、$children、$root、$refs 等   (2.5)（上方事件2）
      initLifecycle(vm)
        
      /**
       * 初始化自定义事件，这里需要注意一点，所有我们在 <comp @click="handleClick" /> 上注册的事件handleClick，监听者不是父组件，而是子组件本身，也就是说事件的派发和监听者都是子组件本身，和父组件无关（谁触发谁监听）
       */
        // 最终是被编译为这样：this.$emit('click'，this.$on('click'，function handleClick() {}))
        // 2.6
        // 这是上面提到的事件3
      initEvents(vm)
        
      // 解析组件的插槽信息，得到 vm.$slot，处理渲染函数，得到 vm.$createElement 方法，即 h 函数
      // 2.7
      // 这是上面提到的事件4
      initRender(vm)
        
      // 调用 beforeCreate 钩子函数（上面的事件5）
      callHook(vm, 'beforeCreate')
        
      // 初始化组件的 inject 配置项，得到 result[key] = val 形式的配置对象，然后对结果数据进行响应式处理，并代理每个 key 到 vm 实例
      // 2.8 （上面的事件6）
      initInjections(vm) // resolve injections before data/props
        
      // 数据响应式的重点，处理 props、methods、data、computed、watch（上面的事件7）
        //具体分析在响应式部分，这里不讲
      initState(vm)
        
      // 解析组件配置项上的 provide 对象，将其挂载到 vm._provided 属性上（上面的事件8）
        // 2.10
      initProvide(vm) // resolve provide after data/props
        
      // 调用 created 钩子函数（上面的事件9）
      callHook(vm, 'created')
  
      // 如果发现配置项上有 el 选项，则自动调用 $mount 方法，也就是说有了 el 选项，就不需要再手动调用 $mount，反之，没有 el 则必须手动调用 $mount(上面的事件10，11)
      if (vm.$options.el) {
        // 调用 $mount 方法，进入挂载阶段
        vm.$mount(vm.$options.el)
      }
    }
  }
  ```

- Vue.component合并到根组件举例

  - ```js
    		Vue.component('comp',{
                template: '<div>I am comp</div>'
            })
    
            new Vue({
                el: '#app',
                data: {
                    msg: 'hello vue'
                }
            })
    
    // 像这样子的组件，其实就会被合并成下面这样子
    		new Vue({
                el: '#app',
                data: {
                    msg: 'hello vue'
                },
                components: {
                    localComp, // 一些本地组件
                    globalComp  // 一些全局组件，如上面用Vue.component创建的comp组件
                }
            })
    ```

- 组件选项合并，发生在三个地方：

  1. Vue.component（CompName，Comp），做了选项合并，合并的Vue内置的全局组件和用户自己的注册的全局组件，最终都会放到全局的components选项中
  2. 子组件内部，{ components：{ xxx } }，局部注册，执行编译器生成的render函数时做了选项合并，会合并全局配置项到组件局部配置项上
  3. 这里的根组件的情况





#### 2.1 initInternalComponent函数

- 第一个参数是vm实例，第二个参数是options配置选项

- ```js
  //    /src/core/instance/init.js
  
  // 性能优化，打平配置对象上的属性，减少运行时原型链的查找，提高自身效率
  export function initInternalComponent(
    vm: Component,
    options: InternalComponentOptions
  ) {
    const opts = (vm.$options = Object.create((vm.constructor as any).options))
    // 把options这个配置对象上的一些属性拿出来，然后复制到上面的$options中
    const parentVnode = options._parentVnode
    opts.parent = options.parent
    opts._parentVnode = parentVnode
  
    const vnodeComponentOptions = parentVnode.componentOptions!
    opts.propsData = vnodeComponentOptions.propsData
    opts._parentListeners = vnodeComponentOptions.listeners
    opts._renderChildren = vnodeComponentOptions.children
    opts._componentTag = vnodeComponentOptions.tag
  
    
    //如果有render函数，将其复制到 vm.$options
    if (options.render) {
      opts.render = options.render
      opts.staticRenderFns = options.staticRenderFns
    }
  }
  ```

  





#### 2.2 resolveConstructorOptions函数

- **从构造函数上解析配置项**

- ```js
  //         /src/core/instance/init.js
  
  /**
   * 从组件构造函数中解析配置对象 options，并合并基类选项
   * @param {*} Ctor 
   * @returns 
   */
  export function resolveConstructorOptions (Ctor: Class<Component>) {
    // 从实例构造函数上获取选项options
    let options = Ctor.options
    if (Ctor.super) {
      // 如果实例构造函数Ctor上有super的话，那就说明存在基类，【递归】解析基类构造函数的选项
      const superOptions = resolveConstructorOptions(Ctor.super)     //这个是super里面的
      
      // 下方cachedSuperOptions缓存了 【基类的配置选项】（这是原Cotr的）
      const cachedSuperOptions = Ctor.superOptions
      
      // 如果 新的选项superOptions 和 缓存选项cachedSuperOptions 不一致的话（判断super的和原来的有没有不同）
      if (superOptions !== cachedSuperOptions) {
        // 说明【基类构造函数的配置项】已经【发生改变】，需要重新设置
        Ctor.superOptions = superOptions
          
        // 检查 Ctor.options 上是否有任何后期修改/附加的选项（＃4976）
        // 找到更改的选项
        //2.4 
        const modifiedOptions = resolveModifiedOptions(Ctor)
        // 如果存在被修改或增加的选项，则合并 修改的选项 和 extend选项 
        if (modifiedOptions) {
          extend(Ctor.extendOptions, modifiedOptions)
        }
          
        // 选项合并，将合并结果赋值为 Ctor.options
        options = Ctor.options = mergeOptions(superOptions, Ctor.extendOptions)
        if (options.name) {
          options.components[options.name] = Ctor
        }
      }
    }
    // 最终返回合并完了的选项，即【从构造函数上解析出来的配置项】 
    return options
  }
  ```

- 传入的参数是**实例的构造函数Ctor**





#### 2.3 mergeOptions函数（选项合并）

- 传入的参数
  - 第一个是父配置选项
  - 第二个是子配置选项
  - 第三个是vm实例

- ```js
  //  /src/core/util/options.js
  
  /**
   * 合并两个选项，出现相同配置项时，子选项会覆盖父选项的配置
   */
  export function mergeOptions (
    parent: Object,
    child: Object,
    vm?: Component
  ): Object {
    if (process.env.NODE_ENV !== 'production') {
      checkComponents(child)
    }
  
    if (typeof child === 'function') {
      child = child.options
    }
  
    // 选项的标准化处理，标准化 props、inject、directive 选项，方便后续程序的处理
    normalizeProps(child, vm)
    normalizeInject(child, vm)
    normalizeDirectives(child)
  
    // 处理原始 child 对象上的 extends 和 mixins，分别执行 mergeOptions，将这些继承而来的选项合并到 parent
    // extends就是继承，mixins就是混入
    // mergeOptions 处理过的对象会含有 _base 属性
      // 如果没有_base，那就进行mergeOptions
    if (!child._base) {
      if (child.extends) {
        parent = mergeOptions(parent, child.extends, vm)
      }
      if (child.mixins) {
        for (let i = 0, l = child.mixins.length; i < l; i++) {
          parent = mergeOptions(parent, child.mixins[i], vm)
        }
      }
    }
  
    // 最后return出去的结果就是这个options
    const options = {}
    let key
    // 遍历 父选项
    for (key in parent) {
      mergeField(key)
    }
  
    // 遍历 子选项，如果父选项不存在该配置，则合并，否则跳过，因为父子拥有同一个属性的情况在上面处理父选项时已经处理过了，用的子选项的值
    for (key in child) {
      if (!hasOwn(parent, key)) {
        mergeField(key)
      }
    }
  
    // 合并选项，childVal 优先级高于 parentVal
    function mergeField (key) {
      // strats = Object.create(null)
      const strat = strats[key] || defaultStrat
      // 值为如果 childVal 存在则优先使用 childVal，否则使用 parentVal
      options[key] = strat(parent[key], child[key], vm, key)
    }
    return options
  }
  ```

  





#### 2.4 resolveModifiedOptions函数

- 传入的参数是  **实例的构造函数**

- 是用来**找到更改的选项**的

- ```js
  //       /src/core/instance/init.js
  
  /**
   * 解析构造函数选项中后续被修改或者增加的选项
   */
  function resolveModifiedOptions (Ctor: Class<Component>): ?Object {
    let modified
    // 构造函数选项
    const latest = Ctor.options
    // 密封的构造函数选项，备份
    const sealed = Ctor.sealedOptions
    // 对比两个选项，记录不一致的选项
    for (const key in latest) {
      if (latest[key] !== sealed[key]) {
        if (!modified) modified = {}
        modified[key] = latest[key]
      }
    }
    return modified
  }
  ```

  





#### 2.5 initLifecycle函数

- 传入的参数是 **实例vm**

- 作用是 **初始化各种属性**

- ```js
  //     /src/core/instance/lifecycle.js
  
  export function initLifecycle(vm: Component) {
    const options = vm.$options
  
    // locate first non-abstract parent
    let parent = options.parent
    if (parent && !options.abstract) {
      while (parent.$options.abstract && parent.$parent) {
        parent = parent.$parent
      }
      parent.$children.push(vm)
    }
  
    vm.$parent = parent
    vm.$root = parent ? parent.$root : vm
  
    vm.$children = []
    vm.$refs = {}
  
    vm._provided = parent ? parent._provided : Object.create(null)
    vm._watcher = null
    vm._inactive = null
    vm._directInactive = false
    vm._isMounted = false
    vm._isDestroyed = false
    vm._isBeingDestroyed = false
  }
  ```





#### 2.6 initEvents函数

- 传入参数为  实例vm

- ```js
  //          /src/core/instance/events.js
  
  export function initEvents(vm: Component) {
    vm._events = Object.create(null)
    vm._hasHookEvent = false
    // init parent attached events
    // 从组件的父配置项上拿到监听器
    const listeners = vm.$options._parentListeners
    if (listeners) {
      updateComponentListeners(vm, listeners)
    }
  }
  
  // 作用就是把 <comp @click="handleClick" /> 最终编译为this.$ emit('click'，this. $on('click'，function handleClick() {}))形式
  //这里具体代码要在this实例方法再学
  ```







#### 2.7 initRender函数

- 接受参数为 实例vm

- 深入讲解在后面学的render，所以这里不讲

- ```js
  //         /src/core/instance/render.js
  
  export function initRender(vm: Component) {
    vm._vnode = null // the root of the child tree
    vm._staticTrees = null // v-once cached trees
    const options = vm.$options
    const parentVnode = (vm.$vnode = options._parentVnode!) // the placeholder node in parent tree
    const renderContext = parentVnode && (parentVnode.context as Component)
    
    // 这是对插槽的处理
    vm.$slots = resolveSlots(options._renderChildren, renderContext)
    vm.$scopedSlots = parentVnode
      ? normalizeScopedSlots(
          vm.$parent!,
          parentVnode.data!.scopedSlots,
          vm.$slots
        )
      : emptyObject
    // bind the createElement fn to this instance
    // so that we get proper render context inside it.
    // args order: tag, data, children, normalizationType, alwaysNormalize
    // internal version is used by render functions compiled from templates
    // @ts-expect-error
      
    // _c方法，参数a，b，c，d分别为标签，属性，孩子，是否是一个标准的选项
    vm._c = (a, b, c, d) => createElement(vm, a, b, c, d, false)
    // normalization is always applied for the public version, used in
    // user-written render functions.
    // @ts-expect-error
    vm.$createElement = (a, b, c, d) => createElement(vm, a, b, c, d, true)
  
    // $attrs & $listeners are exposed for easier HOC creation.
    // they need to be reactive so that HOCs using them are always updated
    const parentData = parentVnode && parentVnode.data
  
    /* istanbul ignore else */
    if (__DEV__) {
      defineReactive(
        vm,
        '$attrs',
        (parentData && parentData.attrs) || emptyObject,
        () => {
          !isUpdatingChildComponent && warn(`$attrs is readonly.`, vm)
        },
        true
      )
      defineReactive(
        vm,
        '$listeners',
        options._parentListeners || emptyObject,
        () => {
          !isUpdatingChildComponent && warn(`$listeners is readonly.`, vm)
        },
        true
      )
    } else {
      defineReactive(
        vm,
        '$attrs',
        (parentData && parentData.attrs) || emptyObject,
        null,
        true
      )
      defineReactive(
        vm,
        '$listeners',
        options._parentListeners || emptyObject,
        null,
        true
      )
    }
  }
  ```





#### 2.8 initInjections函数

- 传入参数为：vm实例

- ```js
  //       /src/core/instance/inject.js
  
  /**
   * 初始化 inject 配置项
   *   1、得到 result[key] = val
   *   2、对结果数据进行响应式处理，代理每个 key 到 vm 实例
   */
  export function initInjections (vm: Component) {
    // 解析 inject 配置项，然后从祖代组件的配置中找到 配置项中每一个 key 对应的 val，最后得到 result[key] = val 的结果
      // 2.9
    const result = resolveInject(vm.$options.inject, vm)
    
    // 对 result 做 数据响应式处理，也有代理 inject 配置中每个 key 到 vm 实例的作用。
    // 不不建议在子组件去更改这些数据，因为一旦祖代组件中 注入的 provide 发生更改，你在组件中做的更改就会被覆盖
    if (result) {
      toggleObserving(false)
      Object.keys(result).forEach(key => {
        /* istanbul ignore else */
        if (process.env.NODE_ENV !== 'production') {
          defineReactive(vm, key, result[key], () => {
            warn(
              `Avoid mutating an injected value directly since the changes will be ` +
              `overwritten whenever the provided component re-renders. ` +
              `injection being mutated: "${key}"`,
              vm
            )
          })
        } else {
            //对解析结果做响应式处理,将每个key代理到vue实例上
          defineReactive(vm, key, result[key])
        }
      })
      toggleObserving(true)
    }
  }
  ```

  





#### 2.9 resolveInject函数

- 传入参数为

  - inject配置项
  - vm实例

- ```js
  //           /src/core/instance/inject.js
  
  /**
   * 解析 inject 配置项，从祖代组件的 provide 配置中找到 key 对应的值，否则用 默认值，最后得到 result[key] = val
   * inject 对象肯定是以下这个结构，因为在 合并 选项时对组件配置对象做了标准化处理
   * @param {*} inject = {
   *  key: {
   *    from: provideKey,
   *    default: xx
   *  }
   * }
   */
  export function resolveInject (inject: any, vm: Component): ?Object {
    if (inject) {
      // inject is :any because flow is not smart enough to figure out cached
      const result = Object.create(null)
      // inject 配置项的所有的 key
      const keys = hasSymbol
        ? Reflect.ownKeys(inject)
        : Object.keys(inject)
  
      // 遍历 inject 选项中 key 组成的数组
      for (let i = 0; i < keys.length; i++) {
        const key = keys[i]
        // 跳过 __ob__ 对象
        // #6574 in case the inject object is observed...
        if (key === '__ob__') continue
          
        // 拿到 provide 中对应的 key(获取from属性)
        const provideKey = inject[key].from
        
        // 从祖代组件的配置项中找到provide选项，从而找到对应key的值
        let source = vm
        // 遍历所有的祖代组件，直到 根组件，找到 provide 中对应 key 的值，最后得到 result[key] = provide[provideKey]
        while (source) {
            //如果source._provided有provideKey的话，就将值给result[key]
          if (source._provided && hasOwn(source._provided, provideKey)) {
            result[key] = source._provided[provideKey]
            break
          }
          // 如果没有的话就向上找，直到找到根组件
          source = source.$parent
        }
          
        // 如果上一个循环未找到(即找到根组件还没有)，则采用 inject[key].default，如果没有设置 default 值，则抛出错误
        if (!source) {
          if ('default' in inject[key]) {
            const provideDefault = inject[key].default
            result[key] = typeof provideDefault === 'function'
              ? provideDefault.call(vm)
              : provideDefault
          } else if (process.env.NODE_ENV !== 'production') {
            warn(`Injection "${key}" not found`, vm)
          }
        }
      }
        // 最终返回解析完成的结果
      return result
    }
  }
  ```





#### 2.10 initProvide函数

- 传入的参数为：vm实例

- ```js
  //              /src/core/instance/inject.js
  
  /**
   * 解析组件配置项上的 provide 对象，将其挂载到 vm._provided 属性上 
   */
  export function initProvide (vm: Component) {
      // 从配置对象上拿到provide选项
    const provide = vm.$options.provide
    if (provide) {
        // 如果provide选项是一个函数的话，就执行它，然后返回一个配置对象，挂载到vm._provided上；否则直接挂载
      vm._provided = typeof provide === 'function'
        ? provide.call(vm)
        : provide
    }
  }
  ```

  







































































