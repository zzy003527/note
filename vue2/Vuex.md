## Vuex

- 理解Vuex

  - vuex是什么

    - 概念：专门在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理，也是一种组件间通信的方式，且适用于任意组件间通信
    - Github地址：https://github.com/vuejs/vuex

  - 什么时候使用Vuex

    - 多个组件依赖于同一状态
    - 来自不同组件的行为需要变更同一状态

  - Vuex工作原理图

    - Vue Components操作，传到Actions成为一个Actions中的方法（发送ajax的时候是在Actions发送）（Actions用来执行判断业务逻辑）
    - 传到Mutations中进行操作执行函数
    - 通过Mutate更新数据到state
    - state渲染Render到Vue Components

    ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1379492/1643034632508-417c8676-569a-41e1-b7db-223fafbedfd7.png)

- 搭建Vuex环境

  - 下载按照vuex： `npm i vuex`（vue3中）

    `npm i vuex@3`(vue2中)

  - 创建`src/store/index.js`该文件用于创建`Vuex`中最为核心的`store`

    ```js
    import Vue from 'vue'
    import Vuex from 'vuex'	// 引入Vuex
    
    Vue.use(Vuex)	// 应用Vuex插件
    
    const actions = {}		// 准备actions——用于响应组件中的动作
    const mutations = {}	// 准备mutations——用于操作数据（state）
    const state = {}			// 准备state——用于存储数据
    
    // 创建并暴露store
    export default new Vuex.Store({
    	actions,
    	mutations,
    	state,
    })
    ```

  - 在`src/main.js`中创建`vm`时传入`store`配置项

    ```js
    import Vue from 'vue'
    import App from './App.vue'
    import store from './store'	// 引入store
    
    Vue.config.productionTip = false
    
    new Vue({
    	el: '#app',
    	render: h => h(App),
    	store,										// 配置项添加store
    	beforeCreate() {
    		Vue.prototype.$bus = this
    	}
    })
    ```

    

- 用vuex编写求和案例

  - ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1379492/1643983901187-53fc6663-8195-463f-99b0-69c406fd5a66.png)

  - **<u>Vuex的基本使用</u>**

    - 初始化数据`state`，配置`actions`、`mutations`，操作文件`store.js`
    - 组件中读取`vuex`中的数据**`$store.state.数据`**
    - 组件中修改`vuex`中的数据**`$store.dispatch('action中的方法名',数据)`或`$store.commit('mutations中的方法名',数据)`**
    - 若没有网络请求或其他业务逻辑，组件中也可越过`actions`，即不写`dispatch`，直接编写`commit`

  - `src/store/index.js`该文件用于创建Vuex中最核心的store

    - ```js
      import Vue from 'vue'
      import Vuex from 'vuex'	// 引入Vuex
      
      Vue.use(Vuex)	// 应用Vuex插件
      
      // 准备actions——用于响应组件中的动作
      const actions = {
      	/* jia(context,value){
      		console.log('actions中的jia被调用了')
      		context.commit('JIA',value)
      	},
      	jian(context,value){
      		console.log('actions中的jian被调用了')
      		context.commit('JIAN',value)
      	}, */
      	jiaOdd(context,value){	// context 相当于精简版的 $store
      		console.log('actions中的jiaOdd被调用了')
      		if(context.state.sum % 2){
      			context.commit('JIA',value)
      		}
      	},
      	jiaWait(context,value){
      		console.log('actions中的jiaWait被调用了')
      		setTimeout(()=>{
      			context.commit('JIA',value)
      		},500)
      	}
      }
      // 准备mutations——用于操作数据（state）
      const mutations = {
      	JIA(state,value){
      		console.log('mutations中的JIA被调用了')
      		state.sum += value
      	},
      	JIAN(state,value){
      		console.log('mutations中的JIAN被调用了')
      		state.sum -= value
      	}
      }
      // 准备state——用于存储数据
      const state = {
      	sum:0 //当前的和
      }
      
      // 创建并暴露store
      export default new Vuex.Store({
      	actions,
      	mutations,
      	state,
      })
      ```

  - `src/components/Count.vue`

    - ```js
      <template>
      	<div>
      		<h1>当前求和为：{{ $store.state.sum }}</h1>
      		<select v-model.number="n">
      			<option value="1">1</option>
      			<option value="2">2</option>
      			<option value="3">3</option>
      		</select>
      		<button @click="increment">+</button>
      		<button @click="decrement">-</button>
      		<button @click="incrementOdd">当前求和为奇数再加</button>
      		<button @click="incrementWait">等一等再加</button>
      	</div>
      </template>
      
      <script>
      	export default {
      		name:'Count',
      		data() {
      			return {
      				n:1, //用户选择的数字
      			}
      		},
      		methods: {
      			increment(){
      				this.$store.commit('JIA',this.n)
      			},
      			decrement(){
      				this.$store.commit('JIAN',this.n)
      			},
      			incrementOdd(){
      				this.$store.dispatch('jiaOdd',this.n)
      			},
      			incrementWait(){
      				this.$store.dispatch('jiaWait',this.n)
      			},
      		}
      	}
      </script>
      
      <style lang="css">button{margin-left: 5px;}</style>
      
      ```

    

- getters配置项

  - 概念：当`state`中的数据需要经过加工后再使用时，可以使用`getters`加工，相当于**全局计算属性**

  - 在`store.js`中追加`getters`配置

    ```js
    const getters = {
    	bigSum(state){
    		return state.sum * 10
    	}
    }
    
    // 创建并暴露store
    export default new Vuex.Store({
    	......
    	getters
    })
    ```

  - 组件中读取数据`$store.getters.bigSum`

    ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1379492/1644051052492-7080e13e-ce76-45d2-9f05-9a5585018837.png)

    ```js
    //         src/store/index.js
    
    import Vue from 'vue'	// 引入Vue核心库
    import Vuex from 'vuex'	// 引入Vuex
    
    Vue.use(Vuex)	// 应用Vuex插件
       
    // 准备actions对象——响应组件中用户的动作
    const actions = {
        addOdd(context,value){
            console.log("actions中的addOdd被调用了")
            if(context.state.sum % 2){context.commit('ADD',value)}
        },
        addWait(context,value){
            console.log("actions中的addWait被调用了")
            setTimeout(()=>{context.commit('ADD',value)},500)
        },
    }
    // 准备mutations对象——修改state中的数据
    const mutations = {
        ADD(state,value){state.sum += value},
        SUB(state,value){state.sum -= value}
    }
    // 准备state对象——保存具体的数据
    const state = {
        sum:0 // 当前的和
    }
    // 准备getters对象——用于将state中的数据进行加工
    const getters = {
        bigSum(){
            return state.sum * 10
        }
    }
       
    //创建并暴露store
    export default new Vuex.Store({
        actions,
        mutations,
        state,
        getters
    })
    ```

  - ```vue
    //            src/Count.vue
    
    <template>
    	<div>
    		<h1>当前求和为：{{ $store.state.sum }}</h1>
    		<h3>当前求和的10倍为：{{ $store.getters.bigSum }}</h3>
    		<select v-model.number="n">
    			<option value="1">1</option>
    			<option value="2">2</option>
    			<option value="3">3</option>
    		</select>
    		<button @click="increment">+</button>
    		<button @click="decrement">-</button>
    		<button @click="incrementOdd">当前求和为奇数再加</button>
    		<button @click="incrementWait">等一等再加</button>
    	</div>
    </template>
    
    <script>
    	export default {
    		name:'Count',
    		data() {
    			return {
    				n:1,
    			}
    		},
    		methods: {
    			increment(){this.$store.commit('ADD',this.n)},
    			decrement(){this.$store.commit('SUBTRACT',this.n)},
    			incrementOdd(){this.$store.dispatch('addOdd',this.n)},
    			incrementWait(){this.$store.dispatch('addWait',this.n)},
    		},
    	}
    </script>
    
    <style>button{margin-left: 5px;}</style>
    ```

    

- 四个map方法的使用

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



























































































