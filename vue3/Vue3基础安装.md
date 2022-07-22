## Vue3基础安装

- 创建Vue3工程

  - 使用vue-cil创建

    - 官方文档：https://cil.vuejs.org/zh/guide/creating-a-project.html#vue-create

    - ```js
      // 查看@vue/cil版本，确保@vue/cil版本在4.5.0以上
      vue ==version
      // 安装或升级@vue-cil
      npm install -g @vue/cil
      // 创建
      vue create vue-test
      // 启动
      cd vue-test
      npm run serve
      ```

  - 使用vite创建

    - 官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

    - vite官网：https://vitejs.cn

    - 什么是vite？——新一代前端构建工具

    - 优势如下：

      - 开发环境中，无需打包操作，可快速的冷启动
      - 轻量快速的热重载（HMR）
      - 真正的按需编译，不再等待整个应用编译完成 

    - ```js
      // 创建工程
      npm init vite-app <project-name>
      // 进入工程目录
      cd <project-name>
      // 安装依赖
      npm install
      // 允许
      npm run dev
      ```

    - 