# 基于Vite的组件库工程化

## 一、使用Vite搭建开发环境

### 1.1	Vite搭建开发环境

- 首先创建一个项目

  ```js
  mkdir my-ui
  cd my-ui
  ```

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



### 1.2 开发一个Vue组件

- 首先安装Vue3

  ```js
  pnpm i vue@3.2.37
  ```

- 编写一个简单的Button组件

  - 编写一个 /src/button/index.ts 组件。

    ```ts
    import { defineComponent, h } from "vue";
    
    export default defineComponent({
    
      name: "SButton",
    
      // template:'<button>MyButton</button>'
    
      render() {
    
        return h("button", null, "MyButton");
    
      },
    
    });
    ```

  - 在 src/index.ts 中启动 Vue 实例。

    ```javascript
    import { createApp } from "vue";
    
    import SButton from "./button";
    
    createApp(SButton).mount("#app");
    ```

  - 还需要在 index.html 中添加一个容器。

    ```bash
    <div id="app"></div>
    ```

    

### 1.3 引入Vite的Vue插件和JSX组件

- 因为Vue3默认不支持模板编译（template语法），所以需要引入插件

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

  - 编写一个 SFC单文件组件 (也就是 .vue 文件) 使用template语法 来测试一下。

    src/SFCButton.vue

    ```xml
    <template>
      <button>SFC Button</button>
    </template>
    
    <script lang="ts">
    export default {
      name: "SFCButton",
    };
    
    </script>
    ```

    引用到 index.ts 中测试一下。

    ```javascript
    import { createApp } from "vue";
    import SFCButton from "./SFCButton.vue";
    
    createApp(SFCButton)
    .mount("#app");
    ```

  - 注意：需要添加一个模块的类型定义来支持.vue类型

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

  - vite.config.ts

    ```javascript
    import vueJsx from '@vitejs/plugin-vue-jsx'
    
    export default defineConfig({
    
      plugins: [
        // 添加JSX插件
        vueJsx({
          // options are passed on to @vue/babel-plugin-jsx
        })
      ],
    
    })
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





### 1.4 库文件封装

- 组件库的形态应该是这样的结构：

  - 可以满足以下的要求：

    - 默认导出为Vue插件；

    - 每个组件可以单独导出。

  - 首先设计一个入口，包含两个功能：

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
  ```

  - 默认 Vite 就是可以支持构建，使用 Vite 的 build 命令就可以打包输出。如果导出的是一个库文件的话，还需要配置【导出模块类型】并确定导出的文件名。

- 配置如下:

  vite.config.ts

  ```php
  const rollupOptions = {
  
    external: ["vue", "vue-router"],
    output: {
      globals: {
        vue: "Vue",
      },
    },
  };
  
  export default defineConfig({
  
    .....  
  
    // 添加库模式配置
  
    build: {
      rollupOptions,
      minify:false,
      lib: {
        entry: "./src/entry.ts",
        name: "SmartyUI",
        fileName: "smarty-ui",
        // 导出模块格式
        formats: ["esm", "umd","iife"],
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

- 测试组件：

  - 首先测试加载全部组件，引用构建完的 smarty-ui.esm.js 文件。

    demo/esm/index.html

    ```xml
    <h1>Demo</h1>
    <div id="app"></div>
    <script type="module">
        import { createApp } from "vue/dist/vue.esm-bundler.js";
        import SmartyUI, { SFCButton, JSXButton, MyButton } from "../../dist/smarty-ui.esm.js";
    
        createApp({
            template: `
          <SButton/>
          <JSXButton/>
          <SFCButton/>
        `}).use(SmartyUI).mount('#app')
    
    </script>
    ```

  - 然后是加载单独组件。

    demo/esm/button.html

    ```xml
    <h1>Demo</h1>
    <div id="app"></div>
    <script type="module">
        import { createApp } from "vue/dist/vue.esm-bundler.js";
        import SmartyUI, {
            SFCButton,
            JSXButton,
            MyButton,
        } from "../../dist/smarty-ui.esm.js";
    
        createApp({
            template: `
    <SButton/>
    <JSXButton/>
    <SFCButton/>
    `,
        })
            .component(SFCButton.name, SFCButton)
            .component(JSXButton.name, JSXButton)
            .component(MyButton.name, MyButton)
            .mount("#app");
    </script>
    ```

    







## 二、用UnoCSS实现原子化CSS

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