### 分享会内容

#### 做todolist遇到的问题

1. element-plus按需引入问题

   开始项目是用vue3+ts

   按需引入需要在vite.config.ts中添加这一段

   ```js
   // vite.config.ts
   import { defineConfig } from 'vite'
   import AutoImport from 'unplugin-auto-import/vite'
   import Components from 'unplugin-vue-components/vite'
   import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
   
   export default defineConfig({
     // ...
     plugins: [
       // ...
       AutoImport({
         resolvers: [ElementPlusResolver()],
       }),
       Components({
         resolvers: [ElementPlusResolver()],
       }),
     ],
   })
   ```

   或者修改Webpack.config.js

   所以改用vue3+ts+vite创建项目

2. element-plus按钮没有办法按下后恢复原来样式

网上的解决方法是：在element按钮组件上面通过click事件来解决

```js
<div>
  <el-button type="primary" icon="el-icon-download" size="mini" @click="handleClick">
      月度报表导出
  </el-button>
</div>

```



```js
handleClick(event) {
        // 点击后鼠标移开恢复按钮默认样式(如果按钮没有加icon图标的话，target.nodeName == "I"可以去掉)
        let target = event.target;
        if(target.nodeName == "I" || target.nodeName == "SPAN"){
            target = event.target.parentNode;
        }
        target.blur();
        //进行其他操作
        ....
}
```

这段代码在vue3中报错，显示event已经被禁用

最后没有解决

3. vue3+ts使用v-for出现unkown问题

   ![1659261427311](images/1659261427311.png)

   在todoThing下面用v-for来遍历了todos

   查找出的解决方法一：

   ​     是tsconfig.json文件配置问题

   ```json
   {
     "compilerOptions": {
       "target": "es5",
       "module": "commonjs",
       "types": [
         "element-plus/global"
       ],
       "isolatedModules": true,
       "esModuleInterop": true,
       "forceConsistentCasingInFileNames": true,
       "strict": true,
       "skipLibCheck": true
     },
     "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
   }
   ```

   由于target: "es5",这个typeScript内置的JS API版本太低，改成 ES2015就行

   结果：没有解决问题

   我的tsconfig.json文件配置

   ```json
   {
     "compilerOptions": {
       "target": "ESNext",
       "useDefineForClassFields": true,
       "module": "ESNext",
       "moduleResolution": "Node",
       "strict": false,
       "jsx": "preserve",
       "sourceMap": true,
       "resolveJsonModule": true,
       "isolatedModules": true,
       "esModuleInterop": true,
       "lib": ["ESNext", "DOM"],
       "skipLibCheck": true
     },
     "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
     "references": [{ "path": "./tsconfig.node.json" }]
   }
   
   ```

   方法二：

   ```js
   <script lang="ts" setup>
   import { defineProps, PropType } from "vue";
   //给todo设定类型限制
   type todo = {
     id: string;
     title: string;
     done: boolean;
   };
   const props = defineProps({
     todos: Array as unknown as PropType<[todo]>,
     newtodos: Array as unknown as PropType<[todo]>,
     default: () => [],
   });
   </script>
   
   
   ```

   

4. 想要引用element-plus图标

   解决方法：使用setup语法糖

### setup语法糖

基本用法

```js
<script setup>
console.log('hello script setup')
</script>
```

之前的写法

```js
<script lang='ts'>
export default {
       setup(){
           return {
           }
       }

                }

</script>
```

里面的代码会被编译成组件 `setup()` 函数的内容。

## 响应式

和从 `setup()` 函数中返回值一样，ref 值在模板中使用的时候会自动解包：

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <button @click="count++">{{ count }}</button>
</template>
```

### 顶层的绑定会被暴露给模板

当使用 `<script setup>` 的时候，任何在 `<script setup>` 声明的顶层的绑定 (包括变量，函数声明，以及 import 引入的内容) 都能在模板中直接使用：

```vue
<script setup>
// 变量
const msg = 'Hello!'

// 函数
function log() {
  console.log(msg)
}
</script>

<template>
  <div @click="log">{{ msg }}</div>
</template>
```



## `defineProps` 和 `defineEmits`

- `defineProps` 和 `defineEmits` 在选项传入后，会提供恰当的类型推断。

### defineprops

在普通props中应用

```vue
export default {
  props: {
    name: String
  },
  setup(props) {
    console.log(props.name)
  }
}
```

第二个参数提供了一个上下文对象，该对象暴露了多个可能在 `setup` 中有用的对象和函数：

```js
const MyComponent = {
  setup(props, context) {
    context.attrs
    context.slots
    context.emit
    context.expose
  }
}
```

`attrs`、`slots` 和 `emit` 分别等同于 [`$attrs`](https://v3.cn.vuejs.org/api/instance-properties.html#attrs)、[`$slots`](https://v3.cn.vuejs.org/api/instance-properties.html#slots) 和 [`$emit`](https://v3.cn.vuejs.org/api/instance-methods.html#emit) 实例 property。





```js
<template>
  <span>{{props.name}}</span>
  // 可省略【props.】
  <span>{{name}}</span>
</template>

<script setup>
  // import { defineProps } from 'vue'
  // defineProps在<script setup>中自动可用，无需导入
  // 需在.eslintrc.js文件中【globals】下配置【defineProps: true】

  // 声明props
  const props = defineProps({
    name: {
      type: String,
      default: ''
    }
  })  
</script>

```

在 `<script setup>` 中必须使用 `defineProps`

### defineemits

子组件（子传父）

```vue
<template>
  <span>{{props.name}}</span>
  // 可省略【props.】
  <span>{{name}}</span>
  <button @click='changeName'>更名</button>
</template>

<script setup>
  // import { defineEmits, defineProps } from 'vue'
  // defineEmits和defineProps在<script setup>中自动可用，无需导入
  // 需在.eslintrc.js文件中【globals】下配置【defineEmits: true】、【defineProps: true】
	
  // 声明props
  const props = defineProps({
    name: {
      type: String,
      default: ''
    }
  }) 
  // 声明事件
  const emit = defineEmits(['updateName'])
  
  const changeName = () => {
    // 执行
    emit('updateName', 'Tom')
  }
</script>

```

父组件

```vue
<template>
  <child :name='state.name' @updateName='updateName'/>  
</template>

<script setup>
  import { reactive } from 'vue'
  // 引入子组件
  import child from './child.vue'

  const state = reactive({
    name: 'Jerry'
  })
  
  // 接收子组件触发的方法
  const updateName = (name) => {
    state.name = name
  }
</script>

```



## 使用组件

在普通的script中使用组件

```js
<script>
	import Demo from './components/Demo'
	export default {
		name: 'App',
		components:{Demo},
		setup(){
			return {
				showHelloMsg
			}
		}
	}
</script>

```



​    在setup里面使用组件，将Demo` 看做被一个变量所引用

```js
<script>
	import Demo from './components/Demo'
</script>
```





### 在vue3中新增的expose函数

新增的 `expose` 是一个函数，该函数允许通过公共组件实例暴露特定的 property。**默认情况下，通过 ref、`$parent` 或 `$root` 获取的公共实例等同于模板所使用的内部实例。**调用 `expose` 将以指定的 property 创建一个独立的实例：

可以通过该实例调用其所有方法和访问其所有数据。

 这样直接准确的拿到一个组件的实例的确很方便，但是有一个很大的问题——父组件能拿到子组件实例的所有属性和方法，那也就意味着子组件是没有满足封装性的

```js
const MyComponent = {
  setup(props, { expose }) {
    const count = ref(0)
    const reset = () => count.value = 0
    const increment = () => count.value++

    // 只有 reset 能被外部访问，例如通过 $refs
    expose({
      reset
    })

    // 在组件内部，模板可以访问 count 和 increment
    return { count, increment }
  }
}
```

## `defineExpose`

使用 `<script setup>` 的组件是**默认关闭**的，也即通过模板 ref 或者 `$parent` 链获取到的组件的公开实例，不会暴露任何在 `<script setup>` 中声明的绑定。

为了在 `<script setup>` 组件中明确要暴露出去的属性，使用 `defineExpose` 编译器宏：

```vue
<script setup>
import { ref } from 'vue'

const a = 1
const b = ref(2)

defineExpose({
  a,
  b
})
</script>
```



当父组件通过模板 ref 的方式获取到当前组件的实例，获取到的实例会像这样 `{ a: number, b: number }` (ref 会和在普通实例中一样被自动解包)











































