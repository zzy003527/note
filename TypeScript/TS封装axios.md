## TS封装axios

- axios的全局配置

  - ```js
    axios.default.baseURL = '基础URL'
    axios.default.timeout = 50000                 //超时时间
    ```

- axios的实例

  - 调用`axios.create()`方法可以创建axios实例

  - ```js
    const Request = axios.create({          //这里的配置只有当使用当前实例时才会生效，而全局配置全局生效
        baseURL: '基础URL'，
        timeout: 50000
    })
    ```

- axios的拦截器

  - axios给我们提供了两大类拦截器

    - 一种是请求方向的拦截（请求成功，请求失败）
    - 另一种是响应方向的拦截（响应成功，响应失败）

  - 拦截器的作用：用于在网络请求的时候在发起请求或者响应时对操作进行响应的处理，发起请求时可以添加网页加载的动画；使用token认证时，判断token状况。响应时可以进行相应的数据处理

  - 请求拦截器：

    ```js
    axios.interceptors.request.use((config) => {
                console.log('进入请求拦截器');
                console.log(config);
                return config           //需要return才能放行
            },(err) => {
                console.log('请求失败');
                console.log(err);
                return err
            })
    
            axios.get('http://www.liulongbin.top:3006/api/getbooks').then((res) => {
                console.log(res);
            })
    
    ```

  - 响应拦截器：

    ```js
    axios.interceptors.response.use((res) => {
                console.log('进入响应拦截器');
                console.log(res.data);
                return res           //需要return才能放行
            },(err) => {
                console.log('响应失败');
                console.log(err);
                return err
            })
    
            axios.get('http://www.liulongbin.top:3006/api/getbooks').then((res) => {
                console.log(res);
            })
    
    ```

    

- 封装代码

  - ```typescript
    //index.ts
    import axios from 'axios'
    import { AxiosInstance, AxiosRequestConfig, AxiosResponse } from 'axios'
    
    // 基础的类封装
    class Request {
      // axios 实例
      instance: AxiosInstance
      // 基础配置，url和超时时间
      baseConfig: AxiosRequestConfig = { 
        baseURL: "http://localhost:3000", 
        timeout: 60000 
    }
    
    constructor(config: AxiosRequestConfig) {
        this.instance = axios.create(Object.assign(this.baseConfig, config))
        
        // 请求拦截(在发送请求之前做什么事)
        this.instance.interceptors.request.use(
          (config: AxiosRequestConfig) => {
            // 一般会请求拦截里面加token
            // const token = localStorage.getItem("token") as string       //类型断言成为string
            // 判断token是否存在，若存在，则加上
            // token && (config.headers!.Authorization = token);  
            return config
          },
          (err: any) => {
            return Promise.reject(err)
          },
        )
        
        // 响应拦截(返回状态判断)
        this.instance.interceptors.response.use(
          (res: AxiosResponse) => {
            // 直接返回res.data
            return res.data
          },
          (err: any) => {
            // 这里用来处理http常见错误，进行全局提示
            let message = "";
            switch (err.response.status) {
              case 400:
                message = "请求错误(400)";
                break;
              case 401:
                message = "未授权，请重新登录(401)";
                // 这里可以做清空storage并跳转到登录页的操作
                break;
              case 403:
                message = "拒绝访问(403)";
                break;
              case 404:
                message = "请求出错(404)";
                break;
              case 408:
                message = "请求超时(408)";
                break;
              case 500:
                message = "服务器错误(500)";
                break;
              case 501:
                message = "服务未实现(501)";
                break;
              case 502:
                message = "网络错误(502)";
                break;
              case 503:
                message = "服务不可用(503)";
                break;
              case 504:
                message = "网络超时(504)";
                break;
              case 505:
                message = "HTTP版本不受支持(505)";
                break;
              default:
                message = `连接出错(${err.response.status})!`;
            }
    
            return Promise.reject(err.response)
          },
        )
      }
      
    
    
      // 定义请求方法(再次封装request方法)
      request<T>(config: AxiosRequestConfig): Promise<T> {
        // return this.instance.request(config)
        return new Promise((resolve,reject) => {
            this.instance
            .request<any, T>(config)
            .then((res) => {
                resolve(res)      //将结果返回出去
            })
            .catch((err) => {
                reject(err)
                return err
            })
        })
      }
    
      get<T>(config: AxiosRequestConfig): Promise<T> {
        // 将传入的参数再加上方法名后调用request
        return this.request<T>({...config, method:'GET' })
      }
    
      post<T>(config: AxiosRequestConfig): Promise<T> {
        return this.request<T>({...config, method:'POST' })
      }
    
      delete<T>(config: AxiosRequestConfig): Promise<T> {
        return this.request<T>({...config, method: 'DELETE' })
      }
    
      patch<T>(config: AxiosRequestConfig): Promise<T> {
        return this.request<T>({...config, method: 'PATCH' })
      }
    
    
    }
    
    export default Request
    ```

  - ```typescript
    // 创建封装后的实例
    import Request from './index'
    const theAxios  = new Request({})
    
    theAxios.request({
        url: '/lyric',
        method: 'get',
        params: { id: 33894312 }
    }).then((res) => {
        console.log(res);
    })
    ```

    