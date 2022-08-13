# Jenkins

- https://mp.weixin.qq.com/s/jPs7kAajOqqnnAzLgswd3Q
  
- 概念
  
  - 持续交付
    - 项目开发
      - 项目的各个升级版本之间间隔实践太长，对用户反馈感知迟钝。无法精确改善用户体验，用户流失严重
      - 需要用**小版本**不断进行**快速迭代**，不断收集用户反馈信息，用最快的速度改进优化
    - 关注点：持续交付的关注的在于研发团队的最新代码能够尽早让最终用户体验到
    - 在 CI 的自动化流程阶段后，运维团队可以快速、轻松地将应用部署到生产环境中或发布给最终使用的用户。
    
  - 持续部署
    - 项目开发
      - 开发过程中进行单元测试能够通过，但是部署到服务器上允许出现问题
      - 这样子单元测试还不够，需要各个模块都能在服务器上运行
    - 关注点：持续部署的关注点在于项目功能**部署至服务器后可以运行**，为下一步测试环境或最终用户正式使用做好准备
    - 从前端的角度考虑，在某些情况下肯定是不能直接通过自动化的方式将最终的 build 结果直接扔到生产机的。持续交互就是可持续性交付供生产使用的的最终 build。最后通过运维或者后端小伙伴进行部署。
    - 作为持续交付的延伸，持续部署可以自动将应用发布到生产环境。
    
  - 持续集成
    - 项目开发
      - 各个小组分别负责各个具体模块开发，本模块独立测试虽然能够通过，但是上线前将所有模块整合到一起集成测试却发现很多问题。
      - 需要经常性、频繁地将所有模块集成在一起进行测试，有问题尽早发现，这就是持续集成
      
    - 关注点：持续集成的关注点在于尽早发现项目整体运行问题，尽早解决
    
    - **持续集成（`Continuous Integration`）是在源代码变更后自动检测、拉取、构建的过程。**
    
      
  
- 部署

  - 准备工作（安装宝塔和LNMP）

    - 首先使用xshell连接到服务器

    - 宝塔安装命令教学：https://www.bt.cn/bbs/thread-19376-1-1.html，**记得配置页面中的开端口教程**

    - centos输入命令：`yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh 12f2c1d72`

    - 下载完毕之后，就可以得到一个地址：宝塔的管理面板

      我腾讯云得到的是

      ```ini
      外网面板地址: http://114.132.164.235:8888/8ce86d5a
      内网面板地址: http://10.0.12.14:8888/8ce86d5a
      username: ybtp0oqx
      password: 343426c6
      If you cannot access the panel,
      release the following panel port [8888] in the security group
      若无法访问面板，请检查防火墙/安全组是否有放行面板[8888]端口
      ```

      阿里云得到的是

      ```ini
      外网面板地址: http://120.77.201.11:8888/23f891dd
      内网面板地址: http://172.20.107.142:8888/23f891dd
      username: nsksp3xz
      password: 3c70c766
      If you cannot access the panel,
      release the following panel port [8888] in the security group
      若无法访问面板，请检查防火墙/安全组是否有放行面板[8888]端口
      ```

    - 然后再登录，登录后在弹出框安装LNMP

      - `LNMP` 其实指的是一套网站运行的服务器架构：L（`Linux`）、N（`Nginx`）、M（`MySQL`）、P（`PHP`）

    - 如果没法登录宝塔页面，可以在终端输入`/etc/init.d/bt default`获取宝塔面板地址

  - 安装jenkins

    - 在服务器远程连接上面装java

      ```bash
      yum install java
      ```

    - 安装下载工具 `wget`

      ```bash
      wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
      ```

    - 导入下载密钥

      ```bash
      rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
      ```

    - 安装 `Jenkins` 

      ```bash
      yum install jenkins
      ```

    

  - 启动jenkins

    - ```bash
      service jenkins start
      ```

      **注：jenkins运行在机器的8080端口，要在防火墙中放行**

    - 在浏览器输入`http://<你的服务器 IP>:8080` 就可以访问到 `Jenkins` 的解锁界面

    - 注意：

      - 此处可能会出现jenkins启动成功但是浏览器无法访问的问题

        - 确定Jenkins是否启动

          ```bash
          systemctl status jenkins
          ```

          可见状态为运行状态

        - 检查防火墙

          ```bash
          systemctl status firewalld
          ```

          可见防火墙正常运行

        - 检查Jenkins端口是否开放

          ```bash
          firewall-cmd --list-ports
          ```

          如果发现没有jenkins的端口，则用

          ```bash
          firewall-cmd --permanent --zone=public --add-port=8080/tcp
          ```

          开启已经配置好的jenkins的端口，然后在重启防火墙，注意一定要重启防火墙！

          ```bash
          systemctl reload firewalld
          ```

        - 记得在服务器里面开启端口

    - 需要输入一段命令来查看密码

      ```bash
      cat /var/lib/jenkins/secrets/initialAdminPassword
      ```

      把控制台输出的密码复制到 `Jenkins` 解锁界面中即可

    - 然后创建用户，得到实例：`http://114.132.164.235:8080/`

      

  - 安装并配置插件

    - 点击`Manage Jenkins`，点击`System Configuration中的Manage Plugins`,选择可选插件，选择`nodejs和Publish Over SSH`安装（左下角`Install without restart`）

    - 点击`Manage Jenkins`,找到`Global Tool Configuration`
      - 配置git：将Git框下的`Path to Git executable`中改为`/usr/bin/git`
      - 配置node：
        - 点击新增NodeJs
        - 在别名中填入`Node_v13.6`
        - 将下方选框中的版本选为`NodeJS 13.6.0`
        - 然后点击应用
      
    - 配置git账户及ssh用户信息
      - 点击`Manage Jenkins`，点击`Configure System`
      - 找到Publish Over SSH
      - 在**Passphrase**处填上**服务器密码**
      - 下方点击新增，在SSH Servers中的**Name和Hostname处填上服务器公网ip**
      - Username填上登录账户（root）
      - Remote Directory填上/
      - 然后点击右下角的**Test Configuration**测试，显示**Success**就说明配置正确
      
    - 配置Github Api插件

      - 到`github`生成`Personal access token`

        - 点击右上角 **头像** - **Settings**，找到 **Developer settings**，然后选中 **Personal access tokens**，点击右上角 **Generate new token**

        - 下方勾选repo和admin：repo_hook

        - 点击 **Generate token** 之后就会生成一段 **token**

        - 注：此token只会显示一次

          ```js
          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
          ```

      - 配置Jenkins中的GitHub配置

        - 既然是要实现代码 `push` 到仓库就自动构建并发布，那么我们肯定得用到 `Webhook`，不过我们不需要手动创建 `Webhook`，插件会帮我们自动创建

          - 在系统配置中，找到 `GitHub` 配置的部分，点击 **添加 `GitHub` 服务器**，点击 **凭据** 下方的 **添加** 按钮，选择 **`Jenkins`**

          - 点击后会弹出一个添加凭据的窗口，**类型** 选择为 **Secret text**，将我们刚才生成的 **Personal access token** 复制到 **Secret** 一栏中，点击添加

          - 添加后我们在 **凭据** 一栏选中 **Secret text**，勾选 **管理 Hook**，点击 **连接测试**，如果正确显示了你的 `GitHub` 用户名，就说明配置成功了

          

  - 创建项目

    - ##### 配置Github项目

      - 点击新建任务，填写任务名称，然后选择第一个（构建一个自由风格的软件项目），确定

      - 勾选 **`GitHub` 项目**，输入 **项目 `URL`**（就是项目的浏览器地址）。将下面的 **源码管理** 选中为 **`Git`**，将你要构建部署的项目的 **`clone`** 地址填到 **Repository URL** 一栏中（就是项目的浏览器地址加上 `.git` 后缀名）

      - **注意：**如果是公开的仓库，`Credentials `一栏可以选择无；如果是私有的仓库，需要先添加一个可以访问该仓库的 `GitHub` 账号，方法类似配置 `GitHub API` 插件，只不过类型一栏选择 用户名密码，然后在下方输入 用户名 密码

    - ##### 配置触发构建器

      - 勾选 **构建触发器** 一栏中的 **GitHub hook trigger for GITScm polling**，勾选 **构建环境** 一栏中的 **Use secret text(s) or file(s)**，在 **凭据** 一栏中选中我们之前添加的 **Secret text**，勾选 **Provide Node & npm bin/ folder to PATH** 为构建项目提供 `Node.js` 环境

    - ##### 增加构建步骤

      - 到 **构建** 一栏中，**增加构建步骤**，选择 **执行 `shell`**，在命令中输入

        ```js
        node -v
        npm -v
        
        rm -rf node_modules
        npm install
        npm run test
        npm run build
        ```

      - 注意：命令中有一条 `npm run test` 命令可以不加，如果是编写好了测试用例的项目，就需要加上，测试代码功能是否正常

      - 如果部署的是原生项目，则不需要加上这些命令，因为原生没有`package.json`文件，无法使用`npm install`去构建依赖

    - ##### 增加构建后的步骤

      - 点击 **增加构建后操作步骤**，选择 **Send build artifacts over SSH**，使用 **SSH** 的方式将代码上传至服务器
      - 在Source files上填写自己想要的目录名（如：todolist/**）
      - 在Remove prefix上填写上面目录名（如：todolist）
      - 在Remote directory填上自己想要储存的目标仓库（如：/www/wwwroot/todolist_project)
      - 在Exec commend填上`service nginx restart`
      - 点击高级，勾上`Clean remote`

    - 注意：

      1. ```js
         Error fetching remote repo 'origin'
         ```

         产生这种报错就尝试清空一下工作空间

      2. ```js
         fatal: The remote end hung up unexpectedly
         ```

         产生这种报错就尝试增加超时时间

         - 在配置的源码管理里，新增高级克隆行为，克隆和拉取时间修改为60分钟

         - 在配置的构建环境中，勾选`Abort the build if it's stuck`，并把超时时间修改为30分钟

      3. ```js
         ERROR: Couldn't find any revision to build. Verify the repository and branch configuration for this job.
         SSH: Current build result is [FAILURE], not going to run.
         ```

         产生这种错误，就去查看自己的github项目在哪个分支上（我的是在main），然后去**项目配置的源码管理的Git的Branch Specifier**上填上自己的分支

    - 

  - 

- 
