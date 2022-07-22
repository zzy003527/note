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

  - **mapState方法**：用于帮助映射`state`中的数据为计算属性

    ```js
    computed: {
      	// 借助mapState生成计算属性：sum、school、subject（对象写法一）
      	...mapState({sum:'sum',school:'school',subject:'subject'}),
    
      	// 借助mapState生成计算属性：sum、school、subject（数组写法二）
      	...mapState(['sum','school','subject']),
    },
    ```

  - **mapGetters方法**：用于帮助映射`getters`中的数据为计算属性

    ```js
    computed: {
        //借助mapGetters生成计算属性：bigSum（对象写法一）
        ...mapGetters({bigSum:'bigSum'}),
    
        //借助mapGetters生成计算属性：bigSum（数组写法二）
        ...mapGetters(['bigSum'])
    },
    ```

  - **mapActions方法**：用于帮助生成`actions`对话的方法，即包含`$store.dispatch(xxx)`的函数

    ```js
    methods:{
        //靠mapActions生成：incrementOdd、incrementWait（对象形式）
        ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
    
        //靠mapActions生成：incrementOdd、incrementWait（数组形式）
        ...mapActions(['jiaOdd','jiaWait'])
    }
    ```

  - **mapMutations方法**：用于帮助生成与`mutations`对话的方法，即包含`$store.commit(xxx)`的函数

    ```js
    methods:{
        //靠mapActions生成：increment、decrement（对象形式）
        ...mapMutations({increment:'JIA',decrement:'JIAN'}),
        
        //靠mapMutations生成：JIA、JIAN（对象形式）
        ...mapMutations(['JIA','JIAN']),
    }
    ```

  - 注意：`mapActions`与`mapMutations`使用时，若需要传递参数需要：**在模板中绑定事件时传递好参数**，否则参数是事件对象

  - ```vue
    //    src/components/Count.vue
    
    <template>
    	<div>
    		<h1>当前求和为：{{ sum }}</h1>
    		<h3>当前求和的10倍为：{{ bigSum }}</h3>
    		<h3>我是{{ name }}，我在{{ school }}学习</h3>
    		<select v-model.number="n">
    			<option value="1">1</option>
    			<option value="2">2</option>
    			<option value="3">3</option>
    		</select>
    		<button @click="increment(n)">+</button>
    		<button @click="decrement(n)">-</button>
    		<button @click="addOdd(n)">当前求和为奇数再加</button>
    		<button @click="addWait(n)">等一等再加</button>
    	</div>
    </template>
    
    <script>
    	import {mapState, mapGetters, mapMutations, mapActions} from 'vuex'	//🔴
    
    	export default {
    		name: 'Count',
    		data() {
    			return {
    				n:1, //用户选择的数字
    			}
    		},
      computed: {		
    			...mapState(['sum','school','name']),
    			...mapGetters(['bigSum'])
    		},
    		methods: {
    			...mapMutations({increment:'ADD', decrement:'SUBTRACT'}),
    			...mapActions(['addOdd', 'addWait'])
    		},
    	}
    </script>
    
    <style>
    	button{
    		margin-left: 5px;
    	}
    </style>
    ```







- 多组件共享数据案例

  - ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1379492/1644055080105-d6e7d6e7-3967-433d-bb8c-b7bdbf92347b.png)

  - ```js
    //   src/App.vue
    
    <template>
      <div>
        <Count><Count/><hr/>
        <Person><Person/>
      </div>
    </template>
    
    <script>
    import Count from "./components/Count.vue";
    import Person from "./components/Person.vue";
    
    export default {
      name: "App",
      components: { Count, Person },
    };
    </script>
    ```

  - ```js
    //         src/store/index.js        用于创建vuex最核心的store
    
    import Vue from 'vue'
    import Vuex from 'vuex'
    
    Vue.use(Vuex)
    
    const actions = {
    	jiaOdd(context,value){
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
    
    //准备mutations——用于操作数据（state）
    const mutations = {
    	JIA(state,value){
    		console.log('mutations中的JIA被调用了')
    		state.sum += value
    	},
    	JIAN(state,value){
    		console.log('mutations中的JIAN被调用了')
    		state.sum -= value
    	},
    	ADD_PERSON(state,value){
    		console.log('mutations中的ADD_PERSON被调用了')
    		state.personList.unshift(value)
    	}
    }
    
    //准备state——用于存储数据
    const state = {
    	sum: 0,
    	school: '尚硅谷',
    	subject: '前端',
    	personList: []
    }
    
    //准备getters——用于将state中的数据进行加工
    const getters = {
    	bigSum(state){
    		return state.sum*10
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

  - ```js
    //           src/components/Count.vue
    
    <template>
    	<div>
    		<h1>当前求和为：{{ sum }}</h1>
    		<h3>当前求和放大10倍为：{{ bigSum }}</h3>
    		<h3>我在{{ school }}，学习{{ subject }}</h3>
    		<h3 style="color:red">Person组件的总人数是：{{ personList.length }}</h3>
    		<select v-model.number="n">
    			<option value="1">1</option>
    			<option value="2">2</option>
    			<option value="3">3</option>
    		</select>
    		<button @click="increment(n)">+</button>
    		<button @click="decrement(n)">-</button>
    		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
    		<button @click="incrementWait(n)">等一等再加</button>
    	</div>
    </template>
    
    <script>
    	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
    	export default {
    		name:'Count',
    		data() {
    			return {
    				n:1, //用户选择的数字
    			}
    		},
    		computed:{
    			...mapState(['sum','school','subject','personList']),
    			...mapGetters(['bigSum'])
    		},
    		methods: {
    			...mapMutations({increment:'JIA',decrement:'JIAN'}),
    			...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
    		}
    	}
    </script>
    
    <style lang="css">button{margin-left: 5px;}</style>
    ```

  - ```js
    //        src/components/Person.vue
    
    <template>
    	<div>
    		<h1>人员列表</h1>
    		<h3 style="color:red">Count组件求和为：{{ sum }}</h3>
    		<input type="text" placeholder="请输入名字" v-model="name">
    		<button @click="add">添加</button>
    		<ul>
    			<li v-for="p in personList" :key="p.id">{{ p.name }}</li>
    		</ul>
    	</div>
    </template>
    
    <script>
    	import {nanoid} from 'nanoid'
      import { mapState } from "vuex"
      
    	export default {
    		name:'Person',
    		data() {
    			return {
    				name:''
    			}
    		},
    		computed:{
    			personList(){return this.$store.state.personList},
    			sum(){return this.$store.state.sum}
    		},
    		methods: {
    			add(){
            if (this.name === "") return
    				const personObj = {id:nanoid(),name:this.name}
    				this.$store.commit('ADD_PERSON',personObj)
    				this.name = ''
    			}
    		},
    	}
    </script>
    ```









- 模块化与命名空间

  - 目的：让代码更好维护，让多种数据分类更加明确

  - 修改`store.js`

    为了解决不同模块命名冲突的问题，将不同模块的`namespaced: true`，之后在不同页面中引入`getter` `action` `mutations`时，需要加上所属的模块名

    ```js
    const countAbout = {
      namespaced: true,	// 开启命名空间
      state: {x:1},
      mutations: { ... },
      actions: { ... },
      getters: {
        bigSum(state){ return state.sum * 10 }
      }
    }
    
    const personAbout = {
      namespaced: true,	// 开启命名空间
      state: { ... },
      mutations: { ... },
      actions: { ... }
    }
    
    const store = new Vuex.Store({
      modules: {
        countAbout,
        personAbout
      }
    })
    ```

  - 开启命名空间后，组件中读取`state`数据

    ```js
    // 方式一：自己直接读取
    this.$store.state.personAbout.list
    // 方式二：借助mapState读取：
    ...mapState('countAbout',['sum','school','subject']),
    ```

  - 开启命名空间后，组件中读取`getters`数据

    ```js
    //方式一：自己直接读取
    this.$store.getters['personAbout/firstPersonName']
    //方式二：借助mapGetters读取：
    ...mapGetters('countAbout',['bigSum'])
    ```

  - 开启命名空间后，组件中调用`dispatch`

    ```js
    //方式一：自己直接dispatch
    this.$store.dispatch('personAbout/addPersonWang',person)
    //方式二：借助mapActions：
    ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
    ```

  - 开启命名空间后，组件中调用`commit`

    ```js
    //方式一：自己直接commit
    this.$store.commit('personAbout/ADD_PERSON',person)
    //方式二：借助mapMutations：
    ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
    ```

    

  - ![img](https://cdn.nlark.com/yuque/0/2022/png/1379492/1643034636483-b8708836-b803-4254-8ca0-5cbee6b77d5e.png?x-oss-process=image%2Fresize%2Cw_449%2Climit_0)

  - ```js
    //      src/store/index.js
    
    import Vue from 'vue'
    import Vuex from 'vuex'
    import countOptions from './count'		// 引入count
    import personOptions from './person'	// 引入person
    
    Vue.use(Vuex)
       
    //创建并暴露store
    export default new Vuex.Store({
        modules:{
            countAbout:countOptions,
            personAbout:personOptions,
        }
    })
    ```

  - ```js
    //          src/store/count.js
    
    export default {
        namespaced:true,
        actions: {
            addOdd(context,value){
                console.log("actions中的addOdd被调用了")
                if(context.state.sum % 2){
                    context.commit('ADD',value)
                }
            },
            addWait(context,value){
                console.log("actions中的addWait被调用了")
                setTimeout(()=>{
                    context.commit('ADD',value)
                },500)
            }
        },
        mutations: {
            ADD(state,value){ state.sum += value },
            SUBTRACT(state,value){ state.sum -= value }
        },
        state: {
            sum:0,
            school:'尚硅谷',
          	subject: '前端'
        },
        getters: {
            bigSum(state){ return state.sum * 10 }
        }
    }
    ```

  - ```js
    //        src/store/person.js
    
    import axios from "axios"
    import { nanoid } from "nanoid"
    
    export default{
        namespaced:true,
        actions:{
            addPersonWang(context,value){
                if(value.name.indexOf('王') === 0){
                    context.commit('ADD_PERSON',value)
                }else{
                    alert('添加的人必须姓王！')
                }
            },
            addPersonServer(context){
                axios.get('http://api.uixsj.cn/hitokoto/get?type=social').then(
                    response => {
                        context.commit('ADD_PERSON',{id:nanoid(),name:response.data})
                    },
                    error => { alert(error.message) }
                )
            }
        },
        mutations:{
            ADD_PERSON(state,value){
                console.log('mutations中的ADD_PERSON被调用了')
                state.personList.unshift(value)
            }
        },
        state:{
            personList:[]
        },
        getters:{
            firstPersonName(state){ return state.personList[0].name }
        }
    }
    ```

  - ```vue
    //          src/components/Count.vue
    
    <template>
    	<div>
    		<h1>当前求和为：{{ sum }}</h1>
    		<h3>当前求和的10倍为：{{ bigSum }}</h3>
    		<h3>我是{{ name }}，我在{{ school }}学习</h3>
    		<h3 style="color:red">Person组件的总人数是：{{ personList.length }}</h3>
    		<select v-model.number="n">
    			<option value="1">1</option>
    			<option value="2">2</option>
    			<option value="3">3</option>
    		</select>
    		<button @click="increment(n)">+</button>
    		<button @click="decrement(n)">-</button>
    		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
    		<button @click="incrementWait(n)">等一等再加</button>
    	</div>
    </template>
    
    <script>
    	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
    
    	export default {
    		name:'Count',
    		data() {
    			return {
    				n:1, // 用户选择的数字
    			}
    		},
        computed:{
    			...mapState('countAbout',['sum','school','name']),
          ...mapState('personAbout',['personList']),
    			...mapGetters('countAbout',['bigSum']),
    		}
    		methods: {
    			...mapMutations('countAbout',{increment:'ADD',decrement:'SUBTRACT'}),
    			...mapActions('countAbout',{incrementOdd:'addOdd',incrementWait:'addWait'})
    		},
    	}
    </script>
    
    <style>button{margin-left: 5px;}</style>
    ```

  - ```vue
    //          src/compoments/Person.vue
    
    <template>
    	<div>
    		<h1>人员列表</h1>
    		<h3 style="color:red">Count组件求和为：{{ sum }}</h3>
            <h3>列表中第一个人的名字是：{{ firstPersonName }}</h3>
    		<input type="text" placeholder="请输入名字" v-model="name">
    		<button @click="add">添加</button>
            <button @click="addWang">添加一个姓王的人</button>
            <button @click="addPerson">随机添加一个人</button>
    		<ul>
    			<li v-for="p in personList" :key="p.id">{{ p.name }}</li>
    		</ul>
    	</div>
    </template>
    
    <script>
    	import {nanoid} from 'nanoid'
    	export default {
    		name: 'Person',
    		data() {
    			return {
    				name:''
    			}
    		},
    		computed: {
    			personList(){
    				return this.$store.state.personAbout.personList
    			},
    			sum(){
    				return this.$store.state.countAbout.sum
    			},
          firstPersonName(){
            return this.$store.getters['personAbout/firstPersonName']
          }
    		},
    		methods: {
    			add(){
    				const personObj = {id:nanoid(),name:this.name}
    				this.$store.commit('personAbout/ADD_PERSON',personObj)
    				this.name = ''
    			},
          addWang(){
            const personObj = {id:nanoid(),name:this.name}
            this.$store.dispatch('personAbout/addPersonWang',personObj)
            this.name = ''   
          },
          addPerson(){
            this.$store.dispatch('personAbout/addPersonServer')
          }
    		},
    	}
    </script>
    ```

    
