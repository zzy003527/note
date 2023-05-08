# vite+vue3+ts+sass

## 一、Vite环境

- 使用pnpm初始软件包配置

  ```js
  npm install -g pnpm
  pnpm init
  ```

- 安装Vite

  ```js
  pnpm i vite@3.0.7 -D
  ```

  - 启动vite为：`npx vite`



## 二、Vue

- 首先安装Vue3

  ```js
  pnpm i vue@3.2.37
  ```

- 安装Vite的Vue插件

  ```js
  pnpm i @vitejs/plugin-vue@"3.0.3" -D
  ```

  - 添加一个 vite.config.ts。

    ```javascript
    import { defineConfig } from "vite";
    import vue from "@vitejs/plugin-vue";
    
    // https://vitejs.dev/config/
    
    export default defineConfig({
    
      plugins: [vue()],
    
    });
    ```

- 添加一个模块的类型定义来支持.vue类型

  - src/shims-vue.d.ts

    ```typescript
    declare module "*.vue" {
      import { DefineComponent } from "vue";
      const component: DefineComponent<{}, {}, any>;
      export default component;
    }
    ```

- 引入JSX组件

  ```js
  pnpm i @vitejs/plugin-vue-jsx@"2.0.0" -D
  ```

  - vite.config.ts改为：

    ```javascript
    import {
        defineConfig
    } from "vite";
    import vue from "@vitejs/plugin-vue";
    import vueJsx from '@vitejs/plugin-vue-jsx';
    // https://vitejs.dev/config/
    
    export default defineConfig({
    
        plugins: [
            vue(),
            vueJsx({})
        ],
    
    });
    ```

  - 需要在tsconfig中配置jsx语法支持

    - ./tsconfig.json

    ```json
    {
        "compilerOptions": {
            "declaration": true, /* 生成相关的 '.d.ts' 文件。 */
            "declarationDir": "./dist/types", /* '.d.ts' 文件输出目录 */
            "jsx": "preserve",
        },
        "include": [
            "./**/*.*",
            "./shims-vue.d.ts",
        ],
        "exclude": [
            "node_modules"
        ],
        "esModuleInterop": true,
        "allowSyntheticDefaultImports": "true"
    }
    ```

- 配置库文件封装

  - 设计一个入口，包含两个功能：

    - 导出全部组件；

    - 实现一个 Vue 插件，插件中编写 install 方法，将所有组件安装到 vue 实例中。
  - /src/entry.ts

  ```javascript
  import { App } from "vue";
  import MyButton from "./button";
  import SFCButton from "./SFCButton.vue";
  import JSXButton from "./JSXButton";
  
  // 导出单独组件
  export { MyButton, SFCButton, JSXButton };
  
  // 编写一个插件，实现一个install方法
  
  export default {
    install(app: App): void {
      app.component(MyButton.name, MyButton);
      app.component(SFCButton.name, SFCButton);
      app.component(JSXButton.name, JSXButton);
    },
  
  };
  
  // 这里是引入组件的地方
  ```

  - 默认 Vite 就是可以支持构建，使用 Vite 的 build 命令就可以打包输出。如果导出的是一个库文件的话，还需要配置【导出模块类型】并确定导出的文件名。

  - vite.config.ts改为：

    ```php
    import {
        defineConfig
    } from "vite";
    import vue from "@vitejs/plugin-vue";
    import vueJsx from '@vitejs/plugin-vue-jsx';
    // https://vitejs.dev/config/
    
    const rollupOptions = {
    
        external: ["vue", "vue-router"],
        output: {
            globals: {
                vue: "Vue",
            },
        },
    };
    
    export default defineConfig({
    
        plugins: [
            vue(),
            vueJsx({})
        ],
        build: {
            rollupOptions,
            minify: false,
            lib: {
                entry: "./src/entry.ts", //这里是入口文件，看你文件在哪就往哪引
                name: "SmartyUI", // 这里是你项目的名字
                fileName: "smarty-ui", //你项目的名字
                // 导出模块格式
                formats: ["esm", "umd", "iife"],
            },
        },
    });
    ```

    接着添加一个 npm 运行脚本，方便运行。

    package.json

    ```json
      "scripts": {
        "build": "vite build"
      },
    pnpm build
    ```

    



## 三、Sass

- ```js
  pnpm i sass -D
  ```

- vite.config.js改为

  ```js
  import {
      defineConfig
  } from "vite";
  import vue from "@vitejs/plugin-vue";
  import vueJsx from '@vitejs/plugin-vue-jsx';
  // https://vitejs.dev/config/
  
  const rollupOptions = {
  
      external: ["vue", "vue-router"],
      output: {
          globals: {
              vue: "Vue",
          },
      },
  };
  
  export default defineConfig({
  
      plugins: [
          vue(),
          vueJsx({})
      ],
      build: {
          rollupOptions,
          minify: false,
          lib: {
              entry: "./src/entry.ts", //这里是入口文件，看你文件在哪就往哪引
              name: "SmartyUI", // 这里是你项目的名字
              fileName: "smarty-ui", //你项目的名字
              // 导出模块格式
              formats: ["esm", "umd", "iife"],
          },
      },
      css: {
          preprocessorOptions: {
              scss: {
                  /**如果引入多个文件，可以使用
                   * '@import "@/assets/scss/globalVariable1.scss";
                   * @import"@/assets/scss/globalVariable2.scss";'
                   **/
                  additionalData: '@import "@/style/globalVar.scss";',
              }
          }
      },
  });
  ```

