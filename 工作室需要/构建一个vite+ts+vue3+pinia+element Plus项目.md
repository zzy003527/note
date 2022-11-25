# 构建一个vite+ts+vue3+pinia+element Plus项目

## 一.搭建Vite项目

- 首先`npm init vite@latest ShanTouLongHu -- --template vue-ts`
- 然后`npm i`



## 二.Pinia状态管理

### 2.1 继续搭建

- 首先`npm install pinia`

- 然后将`main.ts`改为：

  ```typescript
  import { createApp } from 'vue'
  import './style.css'
  import App from './App.vue'
  import { createPinia } from 'pinia'
  
  
  const app = createApp(App);
  app.use(createPinia())
  
  app.mount('#app')
  ```

- **在src下新建store文件夹，新建index.ts**

  - 其中为：

    ```typescript
    import {defineStore} from 'pinia'
    
    export const useStore = defineStore('storeId', {
        state: () => {
            return {
                counter: 0,
                name: 'Eduardo',
                isAdmin: true,
            }
        },
    })
    ```

    



### 2.2 Pinia用法

#### 2.2.1 组件内使用

- ```vue
  <script setup lang="ts">
  import HelloWorld from './components/Hello'
  import {useStore} from "@/store/store";
  import {index} from "@/types";
  
  const store = useStore()
  console.log(store.$state)
  
  
  </script>
  
  <template>
    <div>{{ store.name }}</div>
    <HelloWorld/>
  </template>
  ```

  







## 三.VueRouter

- 首先`npm install vue-router@4`

- **在src下新增router文件夹，里面添加index.ts**

  - ```typescript
    import {createRouter, createWebHistory, RouteRecordRaw} from 'vue-router'
    
    // 这里写组件引入(下面是个例子)
    const Login = () => import('@/pages/login/Login.vue') // 注意这里要带上 文件后缀.vue
    
    const routes: RouteRecordRaw[] = [
        {
            path: '/',
            name: 'Login',
            component: Login,
        },
    ]
    
    const router = createRouter({
        history: createWebHistory(),
        routes,
    })
    
    export default router
    ```

- 修改`main.ts`：

  ```typescript
  import { createApp } from 'vue'
  import './style.css'
  import App from './App.vue'
  import { createPinia } from 'pinia'
  import router from './router'
  
  
  const app = createApp(App);
  app
      .use(createPinia())
      .use(router)
  
  app.mount('#app')
  ```





## 四.Element-ui Plus

- 首先`npm install element-plus --save`

- 然后修改`main.ts`：

  ```typescript
  import { createApp } from 'vue'
  import './style.css'
  import App from './App.vue'
  import { createPinia } from 'pinia'
  import router from './router'
  import ElementPlus from 'element-plus'
  import 'element-plus/dist/index.css'
  
  
  const app = createApp(App);
  app
      .use(createPinia())
      .use(router)
      .use(ElementPlus,{size:'small',zIndex: 3000})
  
  app.mount('#app')
  ```







## 五.Axios封装

- 首先`npm install axios`以及`npm install qs`、`npm i --save-dev @types/qs`

- **然后在src下新建network文件夹**，**network文件夹下有**：

  - `api.ts`：

    ```typescript
    // 配置接口
    
    import {MyRequest} from './request'
    
    import OPTION from "./option";
    
    import { baseUrl } from './ipconfig';
    
    //例子1
      interface Res {
        msg: string
        status: number
        data: any[]
      }
    export function lizi1() {
        return MyRequest<Res>({
            url: baseUrl + OPTION.lizi1,
            method: 'get',
            interceptors: {
          // 请求拦截器
          requestInterceptors: config => {
            console.log('实例请求拦截器1')
      
            return config
          },
          // 响应拦截器
          responseInterceptors: result => {
            console.log('实例响应拦截器1')
            return result
          },
        },
        })
    }
    
    // 例子2
    interface Req {
      bookname: string
      author?: string
      publisher?: string
    }
    interface Res {
      msg: string
      status: number
    }
    export const lizi2 = (data: Req) => {
      return MyRequest<Req, Res>({
        url: baseUrl + OPTION.lizi2,
        method: 'post',
        data,
        interceptors: {
          requestInterceptors(res) {
            console.log('接口请求拦截2')
            return res
          },
          responseInterceptors(result) {
            console.log('接口响应拦截2')
            return result
          },
        },
      })
    }
    ```

  - `ipconfig.ts`:

    ```typescript
    // 配置根路径
    
    const baseUrl = "http://www.liulongbin.top:3006"
    export { baseUrl }
    ```

  - `options.ts`:

    ```typescript
    // 配置接口名
    
    export default {
        // 例子
        lizi1: '/api/getbooks',
        lizi2: '/api/addbook'
    }
    ```

  - `request.ts`：

    ```typescript
    import axios, { AxiosResponse } from 'axios'
    import type { AxiosInstance, AxiosRequestConfig } from 'axios'
    import type {
      RequestConfig,
      RequestInterceptors,
      CancelRequestSource,
    } from './type'
    
    class Request {
      // axios 实例
      instance: AxiosInstance
      // 拦截器对象
      interceptorsObj?: RequestInterceptors<AxiosResponse>
    
      /*
      存放取消方法的集合
      * 在创建请求后将取消请求方法 push 到该集合中
      * 封装一个方法，可以取消请求，传入 url: string|string[]  
      * 在请求之前判断同一URL是否存在，如果存在就取消请求
      */
      cancelRequestSourceList?: CancelRequestSource[]
      /*
      存放所有请求URL的集合
      * 请求之前需要将url push到该集合中
      * 请求完毕后将url从集合中删除
      * 添加在发送请求之前完成，删除在响应之后删除
      */
      requestUrlList?: string[]
    
      constructor(config: RequestConfig) {
        this.requestUrlList = []
        this.cancelRequestSourceList = []
        this.instance = axios.create(config)
        // 参数可以传入interceptors拦截器
        this.interceptorsObj = config.interceptors
        // 拦截器执行顺序 接口请求 -> 实例请求 -> 全局请求 -> 实例响应 -> 全局响应 -> 接口响应
        this.instance.interceptors.request.use(
          (res: AxiosRequestConfig) => res,
          (err: any) => err,
        )
    
        // 使用实例拦截器
        this.instance.interceptors.request.use(
          this.interceptorsObj?.requestInterceptors,
          this.interceptorsObj?.requestInterceptorsCatch,
        )
    
        this.instance.interceptors.response.use(
          this.interceptorsObj?.responseInterceptors,
          this.interceptorsObj?.responseInterceptorsCatch,
        )
        // 全局响应拦截器保证最后执行
        this.instance.interceptors.response.use(
          // 因为我们接口的数据都在res.data下，所以我们直接返回res.data
          (res: AxiosResponse) => {
            return res.data
          },
          (err: any) => err,
        )
      }
      /**
       * @description: 获取指定 url 在 cancelRequestSourceList 中的索引
       * @param {string} url
       * @returns {number} 索引位置
       */
      private getSourceIndex(url: string): number {
        return this.cancelRequestSourceList?.findIndex(
          (item: CancelRequestSource) => {
            return Object.keys(item)[0] === url
          },
        ) as number
      }
      /**
       * @description: 删除 requestUrlList 和 cancelRequestSourceList
       * @param {string} url
       * @returns {*}
       */
      private delUrl(url: string) {
        const urlIndex = this.requestUrlList?.findIndex(u => u === url)
        const sourceIndex = this.getSourceIndex(url)
        // 删除url和cancel方法
        urlIndex !== -1 && this.requestUrlList?.splice(urlIndex as number, 1)
        sourceIndex !== -1 &&
          this.cancelRequestSourceList?.splice(sourceIndex as number, 1)
      }
    
    
      request<T>(config: RequestConfig<T>): Promise<T> {
        return new Promise((resolve, reject) => {
          // 如果我们为单个请求设置拦截器，这里使用单个请求的拦截器
          if (config.interceptors?.requestInterceptors) {
            config = config.interceptors.requestInterceptors(config)
          }
          const url = config.url
          // url存在保存取消请求方法和当前请求url
          if (url) {
            this.requestUrlList?.push(url)
            // TODO 在axios0.22起，对CancelToken已经弃用，需要改成  AbortController 文档：https://axios-http.com/docs/cancellation
            config.cancelToken = new axios.CancelToken(c => {
              this.cancelRequestSourceList?.push({
                [url]: c,
              })
            })
          }
    
          this.instance
            .request<any, T>(config)
            .then(res => {
              // 如果我们为单个响应设置拦截器，这里使用单个响应的拦截器
              if (config.interceptors?.responseInterceptors) {
                res = config.interceptors.responseInterceptors(res)
              }
    
              resolve(res)
            })
            .catch((err: any) => {
              reject(err)
            })
            .finally(() => {
              url && this.delUrl(url)
            })
        })
      }
    
      // 取消请求
      cancelRequest(url: string | string[]) {
        if (typeof url === 'string') {
          // 取消单个请求
          const sourceIndex = this.getSourceIndex(url)
          sourceIndex >= 0 && this.cancelRequestSourceList?.[sourceIndex][url]()
        } else {
          // 存在多个需要取消请求的地址
          url.forEach(u => {
            const sourceIndex = this.getSourceIndex(u)
            sourceIndex >= 0 && this.cancelRequestSourceList?.[sourceIndex][u]()
          })
        }
      }
      // 取消全部请求
      cancelAllRequest() {
        this.cancelRequestSourceList?.forEach(source => {
          const key = Object.keys(source)[0]
          source[key]()
        })
      }
    }
    
    // -------------------------------------------------------------------------------------------------------
    // 这是类实例化
    const request = new Request({
        baseURL: import.meta.env.BASE_URL,
        timeout: 1000 * 60 * 5
      })
    
    // -------------------------------------------------------------------------------------------------------
    // 上面是类，下面封装请求方法
      interface MyRequestConfig<T> extends RequestConfig {
        data?: T
      }
      
      /**
       * @description: 函数的描述
       * @interface D 请求参数的interface
       * @interface T 响应结构的intercept
       * @param {MyRequestConfig} config 不管是GET还是POST请求都使用data
       * @returns {Promise}
       */
      export const MyRequest = <D, T = any>(config: MyRequestConfig<D>) => {
        const { method = 'GET' } = config
        if (method === 'get' || method === 'GET') {
          config.params = config.data
        }
        return request.request<AxiosResponse>(config)
      }
    
    
    
    export default Request
    export { RequestConfig, RequestInterceptors }
    ```

  - `type.ts`:

    ```typescript
    // 类型文件
    import type { AxiosRequestConfig, AxiosResponse } from 'axios'
    export interface RequestInterceptors<T> {
      // 请求拦截
      requestInterceptors?: (config: AxiosRequestConfig) => AxiosRequestConfig
      requestInterceptorsCatch?: (err: any) => any
      // 响应拦截
      responseInterceptors?: (config: T) => T
      responseInterceptorsCatch?: (err: any) => any
    }
    // 自定义传入的参数
    export interface RequestConfig<T = AxiosResponse> extends AxiosRequestConfig {
      interceptors?: RequestInterceptors<T>
    }
    export interface CancelRequestSource {
      [index: string]: () => void
    }
    ```

    



## 六.tsx支持

- 首先`npm install @vitejs/plugin-vue-jsx -D`

- 然后修改`vite.config.ts`：

  ```typescript
  import { defineConfig } from 'vite'
  import vue from '@vitejs/plugin-vue'
  import vueJsx from "@vitejs/plugin-vue-jsx"
  
  // https://vitejs.dev/config/
  export default defineConfig({
    plugins: [
      vue(),
      vueJsx()
    ]
  })
  ```





## 七.环境变量配置

- 项目根目录新建: `.env.development`

  ```
  NODE_ENV=development VITE_APP_WEB_URL= 'YOUR WEB URL'
  ```

- 项目根目录新建:` .env.production`

  ```
  NODE_ENV=production VITE_APP_WEB_URL= 'YOUR WEB URL'
  ```

- 配置 package.json:

  打包区分开发环境和生产环境

  ```
  "scripts": {
      "dev": "vite --open",
      "build:dev": "vite build --mode development",
      "build:pro": "vite build --mode production",
      "preview": "vite preview"
    },
  ```





## 八.Vite 常用基础配置

- 首先`npm install --dev vite-plugin-compression`

- 然后`npm install path`和`npm install --save-dev @types/node`

- 将`vite.config.ts`改为：

  ```typescript
  import {defineConfig} from 'vite'
  import vue from '@vitejs/plugin-vue'
  import vueJsx from '@vitejs/plugin-vue-jsx'
  import * as path from "path"
  import viteCompression from 'vite-plugin-compression'
  
  export default defineConfig({
      base: './', //打包路径
  
      //配置插件
      plugins: [
          vue(),
          vueJsx(),
          viteCompression({
              verbose: true,
              disable: false,
              threshold: 10240,
              algorithm: 'gzip',
              ext: '.gz',
          }),
      ],
      //配置路径别名
      resolve: {
          alias: {
              '@': path.resolve(__dirname, 'src'),
          },
      },
      //配置代理
      server: {
          host: '0.0.0.0',
          port: 3000,
          open: true,
          https: false,
          proxy: {}
      },
  
      // 生产环境打包配置
      //去除 console debugger
      build: {
          minify: 'terser',
          // 进行压缩计算
          sourcemap: false,
          terserOptions: {
            compress: {
              // 打包自动删除console
              drop_console: true,
              drop_debugger: true
            },
            keep_classnames:true,
          },
        },
    
  })
  ```





## 九.最后创建文件夹

- 在src下创建`utils`工具文件夹
- 在src下创建`views`文件夹



