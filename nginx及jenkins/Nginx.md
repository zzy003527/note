# Nginx

-   Nginx是什么

  - `Nginx` (engine x) 是一个**轻量级、高性能的HTTP**和**反向代理服务器**,同时也是一个**通用代理服务器**(TCP/UDP/IMAP/POP3/SMTP),最初由俄罗斯人Igor Sysoev编写。
  - 简单的说：
    - `Nginx`是一个拥有高性能HTTP和反向代理服务器，其特点是`占用内存少`，`并发能力强`，并且在现实中，nginx的并发能力要比在同类型的网页服务器中表现要好
    - `Nginx`专为`性能优化`而开发，最重要的要求便是`性能`，且十分注重效率，有报告nginx能支持高达50000个并发连接数
  - Nginx的作用：
    - Http代理
    - 反向代理

- Nginx的安装（windows）

  - 下载Nginx
    - http://nginx.org/en/download.html
    - 下载后解压
  - 启动Nginx
    - 方法1：直接双击nginx.exe，双击后一个黑色弹窗一闪而过
    - 方法2：到nginx目录下打开cmd命令窗口，输入命令`nginx.exe`，回车即可
  - 检查Nginx是否启动成功
    - 直接在浏览器地址栏输入网址http://localhost:80，出现`Welcome to nginx`即说明启动成功
  - 配置监听
    - **Nginx的配置文件是conf目录下的<u>nginx.conf</u>**，默认配置的nginx监听的端口为80，如果80端口被占用可以修改为未被占用的端口即可
    - 修改是修改server下的listen
    - 当我们修改了nginx的配置文件nginx.conf时，不需要关闭nginx后重新启动nginx，**只需要执行命令`nginx -s reload`即可让改动生效**
  - 关闭Nginx
    - 如果使用cmd命令窗口启动Nginx，关闭cmd窗口是不能结束nginx进程的，可使用两种方法关闭nginx
    - 方法1：输入nginx命令`nginx -s stop`（快速停止nginx）或`nginx -g quit`（完整有序的停止nginx）
    - 方法2：使用taskkill：`taskkill /f /t /im nginx.exe`
    - 注：
      - taskkill是用来终止进程的
      - /f是强制终止
      - /t终止指定的进程和任何由此启动的子进程
      - /im表示指定的进程名称  

  

- Nginx常用命令**(必须进到nginx目录)**

  - 查看nginx的版本号：`nginx -v`

  - 启动：`nginx`  

  - 停止：`nginx -s stop`

  - 安全退出：`nginx -s quit`

  - 重新加载配置文件:`nginx -s reload`

  - 检查nginx是否可以正常运行：`nginx -t`

  - 检查nginx是否安装:`whereis nginx`

  - 查看nginx进程:`ps aux|grep nginx`

  - 注意：如果连接不上，检查阿里云安全组是否开放端口，或者服务器防火墙是否开放端口

    - ```bash
      # 开启
      service firewalld start
      # 重启
      service firewalld restart
      # 关闭
      service firewalld stop
      # 查看防火墙规则
      firewall-cmd --list-all
      # 查询端口是否开放
      firewall-cmd --query-port=8080/tcp
      # 开放80端口
      firewall-cmd --permanent --add-port=80/tcp
      # 移除端口
      firewall-cmd --permanent --remove-port=8080/tcp
      
      #重启防火墙（修改配置后要重启防火墙）
      firewall-cmd --reload
      
      #参数解析
      firwall-cmd  ：是Linux提供的操作firewall的一个工具
      --permanent ：表示设置为持久
      --add-port ：表示添加的端口
      ```

      

  

- Nginx的配置文件

  - 配置文件分为三个模块：

    - 全局块：从配置文件开始到events块之间，主要是设置一些**影响nginx服务器整体运行的配置指令**。

      主要包括：

      - 配置允许Nginx服务器的用户（组）

      - 允许生产的worker process树

      - 进程PID存放路径

      - 日志存放路径和类型

      - 配置文件的引入

      - 比如：

        ```js
        worker_processes 1;
        //这是Nginx服务器并发处理服务的关键配置，worker_processes值越大，可以支持的并发处理了也越多，但是会受到硬件、软件等设备等的制约
        ```

        

    - events块：影响**nginx服务器与用户的网络连接**，常用的设置包括是否开启对多workprocess下的网络连接进行序列化，是否允许同时接收多个网络连接，是否允许同时接收多个网络连接，选取哪种实践驱动模型来处理连接请求，每个word process可以同时支持的最大连接数等

      - 比如：

        ```js
        worker_connections 1024;
        //这就表示每个work_process支持的最大连接数为1024
        ```

    - http块：如反向代理和负载均衡都在此配置

  - http块

    - 这是Nginx服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都放在这里
      - 注意：http块包括**http全局块、server块**
    - http全局块
      - http全局块配置的指令包括文件引入、MIME-TYPE定义、日志自定义、连接超时实践、单链接请求数上线等
    - server块
      - 这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本
      - 每个http块可以包括多个server块，而每个server块就相当于一个虚拟主机
      - 而每个server块也分为全局server块，以及可以同时包含多个location块
      - 全局server块
        - 最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或IP配置
      - location块
        - 一个server块可以配置多个location块
        - 这块的主要作用是基于Nginx服务器接收到的请求字符串（例如server_name/uri-string），对虚拟主机名称（也可以是IP别名）之外的字符串（例如前面的/uri-string）进行匹配，对特定的请求进行处理。地址顶下、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行

    

- Nginx配置文件nginx.conf详解

  - ```ini
    #定义Nginx运行的用户和用户组
    user www www;
     
    #nginx进程数，建议设置为等于CPU总核心数。
    worker_processes 8;
     
    #全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
    error_log /var/log/nginx/error.log info;
     
    #进程文件
    pid /var/run/nginx.pid;
     
    #一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（系统的值ulimit -n）与nginx进程数相除，但是nginx分配请求并不均匀，所以建议与ulimit -n的值保持一致。
    worker_rlimit_nofile 65535;
     
    #工作模式与连接数上限
    events
    {
    	#参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
    	use epoll;
    	#单个进程最大连接数（最大连接数=连接数*进程数）
    	worker_connections 65535;
    }
     
    #设定http服务器
    http
    {
    	include mime.types; #文件扩展名与文件类型映射表
    	default_type application/octet-stream; #默认文件类型
    	#charset utf-8; #默认编码
    	server_names_hash_bucket_size 128; #服务器名字的hash表大小
    	client_header_buffer_size 32k; #上传文件大小限制
    	large_client_header_buffers 4 64k; #设定请求缓
    	client_max_body_size 8m; #设定请求缓
    	sendfile on; #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
    	autoindex on; #开启目录列表访问，合适下载服务器，默认关闭。
    	tcp_nopush on; #防止网络阻塞
    	tcp_nodelay on; #防止网络阻塞
    	keepalive_timeout 120; #长连接超时时间，单位是秒
     
    	#FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。下面参数看字面意思都能理解。
    	fastcgi_connect_timeout 300;
    	fastcgi_send_timeout 300;
    	fastcgi_read_timeout 300;
    	fastcgi_buffer_size 64k;
    	fastcgi_buffers 4 64k;
    	fastcgi_busy_buffers_size 128k;
    	fastcgi_temp_file_write_size 128k;
     
    	#gzip模块设置
    	gzip on; #开启gzip压缩输出
    	gzip_min_length 1k; #最小压缩文件大小
    	gzip_buffers 4 16k; #压缩缓冲区
    	gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    	gzip_comp_level 2; #压缩等级
    	gzip_types text/plain application/x-javascript text/css application/xml;
    	#压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    	gzip_vary on;
    	#limit_zone crawler $binary_remote_addr 10m; #开启限制IP连接数的时候需要使用
     
    	upstream blog.ha97.com {
    		#upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
    		server 192.168.80.121:80 weight=3;
    		server 192.168.80.122:80 weight=2;
    		server 192.168.80.123:80 weight=3;
    	}
     
    	#虚拟主机的配置
    	server
    	{
    		#监听端口
    		listen 80;
    		#域名可以有多个，用空格隔开
    		server_name www.clanfei.com clanfei.com;
    		index index.html index.htm index.php;
    		root /data/www/clanfei;
    		location ~ .*.(php|php5)?$
    		{
    			fastcgi_pass 127.0.0.1:9000;
    			fastcgi_index index.php;
    			include fastcgi.conf;
    		}
    		#图片缓存时间设置
    		location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$
    		{
    			expires 10d;
    		}
    		#JS和CSS缓存时间设置
    		location ~ .*.(js|css)?$
    		{
    			expires 1h;
    		}
    		#日志格式设定
    		log_format access '$remote_addr - $remote_user [$time_local] "$request" '
    		'$status $body_bytes_sent "$http_referer" '
    		'"$http_user_agent" $http_x_forwarded_for';
    		#定义本虚拟主机的访问日志
    		access_log /var/log/nginx/access.log access;
     
    		#对 "/" 启用反向代理
    		location / {
    			proxy_pass http://127.0.0.1:88;
    			proxy_redirect off;
    			proxy_set_header X-Real-IP $remote_addr;
    			#后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
    			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    			#以下是一些反向代理的配置，可选。
    			proxy_set_header Host $host;
    			client_max_body_size 10m; #允许客户端请求的最大单文件字节数
    			client_body_buffer_size 128k; #缓冲区代理缓冲用户端请求的最大字节数，
    			proxy_connect_timeout 90; #nginx跟后端服务器连接超时时间(代理连接超时)
    			proxy_send_timeout 90; #后端服务器数据回传时间(代理发送超时)
    			proxy_read_timeout 90; #连接成功后，后端服务器响应时间(代理接收超时)
    			proxy_buffer_size 4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
    			proxy_buffers 4 32k; #proxy_buffers缓冲区，网页平均在32k以下的设置
    			proxy_busy_buffers_size 64k; #高负荷下缓冲大小（proxy_buffers*2）
    			proxy_temp_file_write_size 64k;
    			#设定缓存文件夹大小，大于这个值，将从upstream服务器传
    		}
     
    		#设定查看Nginx状态的地址
    		location /NginxStatus {
    			stub_status on;
    			access_log on;
    			auth_basic "NginxStatus";
    			auth_basic_user_file conf/htpasswd;
    			#htpasswd文件的内容可以用apache提供的htpasswd工具来产生。
    		}
     
    		#本地动静分离反向代理配置
    		#所有jsp的页面均交由tomcat或resin处理
    		location ~ .(jsp|jspx|do)?$ {
    			proxy_set_header Host $host;
    			proxy_set_header X-Real-IP $remote_addr;
    			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    			proxy_pass http://127.0.0.1:8080;
    		}
    		
    		#所有静态文件由nginx直接读取不经过tomcat或resin
    		location ~ .*.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)$
    		{
    			expires 15d;
    		}
    		location ~ .*.(js|css)?$
    		{
    			expires 1h;
    		}
    	}
    }
    ```

    

- 正向代理和反向代理

  - 正向代理
    - 局域网中的电脑用户想要直接访问网络是不可行的，只能通过代理服务器（Server）来访问，这种代理服务就被称为正向代理。
    - 即正向代理的对象是客户端，服务器端看不到真正的客户端。![img](http://hungryturbo.gitee.io/webcanteen/images/%20forwardproxy.jpg)
    - 配置：
    
      ```bash
      resolver 8.8.8.8 # 谷歌的域名解析地址
      server {
       location / {
            # 当客户端请求我的时候，我会把请求转发给它
            # $http_host 要访问的主机名 $request_uri 请求路径
            proxy_pass http://$http_host$request_uri;
       }
      }
      ```
    - 
    - d
    
  - 反向代理
    - 客户端无法感知代理，因为客户端访问网络不需要配置，只要把请求发送到反向代理服务器，由**反向代理服务器去选择目标服务器**获取数据，然后再返回到客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器IP地址。
    
    - 即反向代理的对象是服务端，客户端看不到真正的服务端。![img](http://hungryturbo.gitee.io/webcanteen/images/reverseproxy.jpg)
    
    - 配置：
    
      - 在`Windows/System32/drivers/etc/hosts`中配置：
    
        ```bash
        127.0.0.1       nginx.deeruby.com
        # 后面那个就是www.123.com
        ```
    
      - 在nginx.conf中配置：
    
        ```bash
        server {
          listen       80;
          server_name  nginx.deeruby.com;
        
          location / {
            proxy_pass http://127.0.0.1:8080;
          }
          
           location /api {
              proxy_pass http://127.0.0.1:3002;
              proxy_set_header Host $host;
          }
        }
        ```
    
      - 其含义为用户访问` nginx.deeruby.com` 的时候，将其反向代理到`127.0.0.1:8080`上（前端部分）；访问 `nginx.deeruby.com/api`的时候，将其反向代理到 `127.0.0.1:3002/api` 上（后端部分）；此时前后端同域，**以此解决跨域问题。**
    
      - `proxy_set_header Host $host;`    ：用来在后端获取用户发送过来的请求头
    
        

- 负载均衡

  - 是高可用网络基础架构的关键组件，通常用于将工作**负载分布到多个服务器**来提高网站、应用、数据库或其他服务的性能和可靠性。

  - 如果没有负载均衡，客户端与服务端的操作通常是：**客户端请求服务端，然后服务端去数据库查询数据，将返回的数据带给客户端**：![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b03812eb40b047be8052ee9288f6798e~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

  - 但随着客户端越来越多，数据，访问量飞速增长，这种情况显然无法满足，我们从上图发现，客户端的请求和相应都是通过服务端的，那么我们加大服务端的量，让多个服务端分担，是不是就能解决这个问题了呢？

  - 但此时对于客户端而言，他去访问这个地址就是固定的，才不会去管那个服务端有时间，你只要给我返回出数据就OK了，所以我们就需要一个“管理者“，将这些服务端找个老大过来，客户端直接找老大，再由老大分配谁处理谁的数据，从而减轻服务端的压力，而这个”老大“就是**反向代理服务器**，而端口号就是这些服务端的工号。![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c6c634ec69f54d5ab76644d8dd78b0c2~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

    

  - 配置：

    ```bash
    # upstream 指定后端服务器地址
    # weight 设置权重
    # server 中会将 http://webcanteen 的请求转发到 upstream 池中
    upstream webcanteen {
        server 127.0.0.1:66 weight=10;
        server 127.0.0.1:77 weight=1;
        server 127.0.0.1:88 weight=1;
    }
    server {
        location / {
            proxy_pass http://webcanteen
        }
    }
    ```

    

- 分配方式

  - 轮询(默认），每个请求按照时间顺序轮流分配到不同的后端服务器，如果某台后端服务器宕机，Nginx 轮询列表会自动将它去除掉。

  - weight(加权轮询)，轮询的加强版，weight 和访问几率成正比，主要用于后端服务器性能不均的场景。

  - ip_hash，每个请求按照访问 IP 的 hash 结果分配，这样每个访问可以固定访问一个后端服务器。

  - url_hash，按照访问 URL 的 hash 结果来分配请求，使得每个URL定向到同一个后端服务器上，主要应用于后端服务器为缓存时的场景。

  - 自定义hash，基于任意关键字作为 hash key 实现 hash 算法的负载均衡

  - fair，按照后端服务器的响应时间来分配请求，响应时间短则优先分配。

    

- 动静分离

  - 在外面的开发中，有些请求是需要后台处理的，有些请求是不需要经过后台处理的（如：css、html、jpg、js等文件），这些不需要经过后台处理的文件成为静态文件，让动态网址里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，外面就可以根据静态资源的特点将其做缓存操作，提高资源响应的速度

  - 也就是说，我们将动态资源和静态资源分离出来，交给不同的服务器去解析，这样就加快了解析的速度，从而降低由单个服务器的压力![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a91e7a373df14e90891e6f4f62a629d7~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

  - Nginx动静分离简单来说就是把动态跟静态请求分开，不能理解成只是单纯的把动态页面和静态页面物理分离。严格意义上说应该是动态请求跟静态请求分开，可以理解成使用Nginx处理静态页面，Tomcat 处理动态页面

  - 动静分离从目前实现角度来讲大致分为两种，一种是纯粹把静态文件独立成单独的域名，放在独立的服务器上,也是目前主流推崇的方案;

    另外一种方法就是动态跟静态文件混合在一起发布， 通过nginx来分开

  - 通过location指定不同的后缀名实现不同的请求转发。通过expires参数设置，可以使浏览器缓存过期时间，减少与服务器之前的请求和流量。

  - 具体Expires 定义:是给一个资源设定一个过期时间，也就是说无需去服务端验证,直接通过浏览器自身确认是否过期即可，所以不会产生额外的流量。此种方法非常适合不经常变动的资源

  - 配置例子：

    - `mkdir data`创建一个data目录，`cd data`进入data目录，`mkdir www`和`mkdir image`创建两个目录

    - 通过`vim nginx.conf`进入配置文件

    - 在`server`节点中的`server_name`填写当前要访问的ip

    - 在下面配置location：

      ```bash
      location /www/ {
         root /data/;
         index index.html index.htm;
      }
      
      location /image/ {
          root /data/;
          autoindex on;
      }
      ```

      

- 

- 

- 
