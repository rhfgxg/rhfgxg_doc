## 日常维护

## 使用
### 新建nginx 用户及用户组
```bash
# 以 root 用户登陆
groupadd nginx
useradd -g nginx -M nginx
# useradd命令的-M参数用于不为nginx建立home目录

# 修改/etc/passwd，使得nginx用户无法bash登陆
vim /etc/passwd
# 找到 nginx 用户 将后面/bin/bash改为/sbin/nologin
```

### 启动和关闭 nginx
```bash
start nginx   启动nginx
nginx -s stop 快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。 
nginx -s quit 平稳关闭Nginx，保存相关信息，有安排的结束web服务。 
nginx -s reload 因改变了Nginx相关配置，需要重新加载配置而重载。 
nginx -s reopen 重新打开日志文件。
nginx -c filename 为 Nginx 指定一个配置文件，来代替缺省的。 
nginx -t 不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。 
nginx -v 显示 nginx 的版本。 
nginx -V 显示 nginx 的版本，编译器版本和配置参数

#关闭单个进程
kill -s QUIT 1628
# 快速停止 Nginx：
kill -TERM 主进程号
# 强制停止 Nginx：
pkill -9 nginx
```

### 获取所有运行的nginx进程的列表：ps -ax | grep nginx
```bash
ps -ax | grep nginx

列表中 master 进程是主进程
第二列是 进程的进程号
```

### 修改配置文件
使用文本编辑器对配置文件进行编辑
```bash
sudo vim *.conf   # 使用 vim 对配置文件进行编辑

sudo nginx -t     # 检查配置文件语法错误，返回 ok
sudo nginx -s reload  # 重启 nginx，使配置生效
```

## 关键文件路径（自用）
下面路径并不是默认路径，而是本人使用的服务器文件路径速查表

### 运行文件路径
```bash
/usr/sbin/nginx
```

### 网页文件路径
```bash
/var/www/index.html   # 首页文件
/var/www/50x.html     # 错误页面文件
/var/www/data/images/   # 图片文件夹

/usr/share/nginx/html  #  默认网页文件

# 用来测试的网页
/var/www/ceshi/index.html   # 主页面
/var/www/ceshi/aaa/aaa.html
/var/www/ceshi/bbb/bbb.html
/var/www/ceshi/ccc/ccc.html
/var/www/ceshi/images    # 静态图片文件夹

├── html                                              这是编译安装时nginx的默认站点目录，类似Apache的默认站点htdocs  
│   ├── 50x.html                                  错误页面优雅替代显示文件，例如：出现502错误时会调用此页面error_page 500 502 503 504 /50x.html
│   └── index.html                               默认的首页文件，index.html\index.asp\index.jsp来做网站的首页文件
```

### 配置文件路径
```bash
/etc/nginx/nginx.conf   # 配置文件
/etc/nginx/sites-enabled/  # 配置文件
/etc/nginx/conf.d/*.conf;   # 加载子配置项

├── conf                                              这是nginx所有配置文件的目录
│   ├── fastcgi.conf                                      fastcgi 相关参数的配置文件
│   ├── fastcgi.conf.default                          fastcgi.conf 的原始备份
│   ├── fastcgi_params                                fastcgi的参数文件
│   ├── fastcgi_params.default
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types                                    媒体类型
│   ├── mime.types.default
│   ├── nginx.conf                                    nginx默认的主配置文件
│   ├── nginx.conf.default
│   ├── scgi_params                                scgi 相关参数文件
│   ├── scgi_params.default
│   ├── uwsgi_params                            uwsgi相关参数文件
│   ├── uwsgi_params.default
│   └── win-utf

├── fastcgi_temp                                 fastcgi临时数据目录
```

### 日志文件路径
```bash
/var/log/nginx/access.log  main;   # 访问日志
/var/log/nginx/error.log warn;   # 错误日志
/var/run/nginx.pid;      # Nginx 服务启动时 所有进程的 id号
```

### 其他关键文件路径
```bash
/etc/nginx/mime.types;      # 文件扩展名与类型映射表

├── nginx-1.6.3 -> /application/nginx-1.6.3
├── proxy_temp                                临时目录
├── sbin                                            这是nginx命令的目录，如nginx的启动命令nginx
│   ├── nginx                                    Nginx的启动命令nginx
│   └── nginx.old
├── scgi_temp                                 临时目录
└── uwsgi_temp                              临时目录
```

### pcre文件路径
```bash
/usr/local/src/pcre-8.45
```

## Nginx 反向代理服务器
### Nginx 运行结构
nginx工作( worker)码包括核心和功能模块
<img width="720" height="447" alt="image" src="https://github.com/user-attachments/assets/d5467815-add4-4306-8eb4-88e50b49282b" />
模块构成了大部分的演示和应用层功能
模块类型：核心模块，事件模块，阶段处理程序，协议，可变处理程序，过滤器，上游和负载平衡器
模块读取和写入网络和存储，转换内容，执行出站过滤，
应用服务器端包含操作，并在代理启动时将请求传递给上游服务器
nginx不支持动态加载的模块; 即在构建阶段将模块与核心一起编译

### 工作模式
创建工作进程，创建监听套接字（监听http请求）
nginx有一个主进程和几个工作进程。 主进程的主要目的是读取和评估配置，并维护工作进程。 工作进程对请求进行实际处理
监听到的请求由工作进程处理
事件循环：包括全面的内部调用，并且在很大程度上依赖异步任务处理的想法
异步操作通过模块化，事件通知，广泛使用回调函数和微调定时器来实现
nginx的作用是检查网络和存储的状态，初始化新连接，将其添加到运行循环中，并异步处理直到完成，此时连接被重新分配并从运行循环中删除，nginx不会链接进程，所以没有进程的创建销毁和跳转，更加高效
任务结束，将链接从循环中删除

### 配置文件
#### 查看配置信息
可以使用 nginx -V 查看Nginx 版本和配置参数信息
```bash
nginx -V 2>&1 | sed 's/ /\n/g'
通过管道对输出信息做格式整理
```

#### 文件格式
```bash
nginx由配置文件中指定的指令控制的模块组成。 
指令分为指令语句 和 块指令。 

指令语句：由空格分隔的 名称和参数 组成，并以分号 ; 结尾。 
块指令：以大括号 {} 包围的一组附加指令，不以分号结尾。 
```

#### 配置文件结构
```bash
user nobody; # 全局有效的指令区：main

events {
    # 处理连接的配置
}

http {

    # 作用于 HTTP 连接，并影响所有虚拟服务器的配置

    server {
        # HTTP 虚拟服务器 1 的配置

        location /one {
            # 响应 URI /one 的连接配置
        }

        location /two {
            # 响应 URI /two 的连接配置
        }
    }

    server {
        # HTTP 虚拟服务器 2 的配置
    }
}

stream {
    # 响应 TCP 协议连接 并影响所有虚拟服务器的配置

    server {
        # TPC 虚拟服务器 1 的配置
    }
}
mail {
    server {
    
    }
    server {
    
    }
}
```
为了使配置更易于维护，建议您将特定功能整合成一个配置文件存储在 /etc/nginx/conf.d目录中，
然后在主 nginx.conf文件中使用 include指令引用(包函)指定文件的内容
```bash
include conf.d/http; 
include conf.d/stream; 
include conf.d/exchange-enhanced;
```
通常是将所有的虚拟主机配置文件（也就是 server 配置块的内容）存放在 /etc/nginx/conf.d/ 或者 /etc/nginx/sites-enabled/ 目录中，在主配置文件中已经默认声明了会读取这两个文件夹下所有 *.conf 文件。
```bash
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```

#### 配置文件内容
```bash
server {
    listen 8070 default_server;

    root /var/www/;

    index index.html index.htm;

    server_name localhost;

    location / {
        try_files $uri $uri/ =404;
    }
}

 user  nginx;                        # 运行用户，默认即是nginx，可以不进行设置
 worker_processes  1;                # Nginx 进程数，一般设置为和 CPU 核数一样
 error_log  /var/log/nginx/error.log warn;   # Nginx 的错误日志存放目录
 pid        /var/run/nginx.pid;      # Nginx 服务启动时的 pid 存放位置

 events {
    use epoll;     # 使用epoll的I/O模型(如果你不知道Nginx该使用哪种轮询方法，会自动选择一个最适合你操作系统的)
    worker_connections 1024;   # 每个进程允许最大并发数
}

 http {   # 配置使用最频繁的部分，代理、缓存、日志定义等绝大多数功能和第三方模块的配置都在这里设置
    # 设置日志模式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;   # Nginx访问日志存放位置

    sendfile            on;   # 开启高效传输模式
    tcp_nopush          on;   # 减少网络报文段的数量
    tcp_nodelay         on;
    keepalive_timeout   65;   # 保持连接的时间，也叫超时时间，单位秒
    types_hash_max_size 2048;
    include             /etc/nginx/mime.types;      # 文件扩展名与类型映射表
    default_type        application/octet-stream;   # 默认文件类型

    include /etc/nginx/conf.d/*.conf;   # 加载子配置文件

    server {
        listen       80;       # 配置监听的端口
        server_name  localhost;    # 配置的域名

        location / {
            root   /usr/www;  # 网站根目录
            index  index.html index.htm;   # 默认首页文件
            deny 172.168.22.11;   # 禁止访问的ip地址，可以为all（所有）
           allow 172.168.33.44； # 允许访问的ip地址，可以为all
     }

        error_page 500 502 503 504 /50x.html;  # 默认（错误代码）50x对应的访问页面
        error_page 400 404 error.html;   # 同上
    }
 }
```

##### 内容解析
###### 全局块 main
全局块是默认配置文件从开始到events块之间的一部分内容，主要设置一些影响Nginx服务器整体运行的配置指令， 因此，这些指令的作用域是Nginx服务器全局。
```bash
指定可以运行nginx服务的用户和用户组，只能在全局块配置
# user [user] [group]
将user指令注释掉，或者配置成nobody的话所有用户都可以运行
# user nobody nobody;
user指令在Windows上不生效，如果你制定具体用户和用户组会报小面警告
# nginx: [warn] "user" is not supported, ignored in D:\software\nginx-1.18.0/conf/nginx.conf:2

指定工作线程数，可以制定具体的进程数，也可使用自动模式，这个指令只能在全局块配置
# worker_processes number | auto；
列子：指定4个工作线程，这种情况下会生成一个master进程和4个worker进程
# worker_processes 4;

指定pid文件存放的路径，这个指令只能在全局块配置
# pid logs/nginx.pid;

指定错误日志的路径和日志级别，此指令可以在全局块、http块、server块以及location块中配置
其中debug级别的日志需要编译时使用--with-debug开启debug开关
# error_log [path] [debug | info | notice | warn | error | crit | alert | emerg] 
# error_log 路径 等级
# error_log  logs/error.log  notice;
# error_log  logs/error.log  info;

包含别的配置文件
inclde /etc/nginx/modules-enadled/*.conf;
```

##### events 块
events块涉及的指令主要影响Nginx服务器与用户的网络连接。
常用到的设置包括：是否开启对多worker process下的网络连接进行序列化，是否允许同时接收多个网络连接， 选取哪种事件驱动模型处理连接请求，每个worker process可以同时支持的最大连接数等。
```bash
当某一时刻只有一个网络连接到来时，多个睡眠进程会被同时叫醒，但只有一个进程可获得连接。
如果每次唤醒的进程数目太多，会影响一部分系统性能。在Nginx服务器的多进程下，就有可能出现这样的问题。

开启的时候，将会对多个Nginx进程接收连接进行序列化，防止多个进程对连接的争抢
默认是开启状态，只能在events块中进行配置
 #accept_mutex on | off;

关闭时，nginx一个工作进程只能同时接受一个新的连接。否则，一个工作进程可以同时接受所有的新连接。 
如果nginx使用kqueue连接方法，那么这条指令会被忽略，因为这个方法会报告在等待被接受的新连接的数量。
默认是off状态，只能在event块配置
# multi_accept on | off;

指定使用哪种网络IO模型
可选择的内容有：select、poll、kqueue、epoll、rtsig、/dev/poll以及eventport，
  一般操作系统不是支持上面所有模型的。
只能在events块中进行配置
# use 模型名
# use epoll

设置每个工作进程同时开启的最大连接数，当每个工作进程接受的连接数超过这个值时将不再接收连接
当所有的工作进程都接收满时，连接进入logback，logback满后连接被拒绝   logbck(日志文件)
只能在events块中进行配置
注意：这个值不能超过超过系统支持打开的最大文件数，也不能超过单个进程支持打开的最大文件数，
  具体可以参考这篇文章：https://cloud.tencent.com/developer/article/1114773
# worker_connections  数字;
```

##### http 块
http块是Nginx服务器配置中的重要部分，代理、缓存和日志定义等绝大多数的功能和第三方模块的配置都可以放在这个模块中。
```bash
常用的浏览器中，可以显示的内容有HTML、XML、GIF及Flash等种类繁多的文本、媒体等资源，
浏览器为区分这些资源，需要使用MIME Type。
换言之，MIME Type是网络资源的媒体类型。Nginx服务器作为Web服务器，必须能够识别前端请求的资源类型。

include指令，用于包含其他的配置文件，可以放在配置文件的任何地方，
但是要注意你包含进来的配置文件一定符合配置规范，
下面的指令将mime.types包含进来，mime.types和ngin.cfg同级目录，不同级的话需要指定具体路径
# include  mime.types;

配置默认类型，如果不加此指令，默认值为text/plain。
此指令还可以在http块、server块或者location块中进行配置。
# default_type  application/octet-stream;

记录Nginx服务器提供服务过程应答前端请求的日志，与error_log类似
access_log配置，此指令可以在http块、server块或者location块中进行设置
# access_log 路径 [format [buffer=size]]

关闭access_log
# access_log off;

log_format指令，用于定义日志格式，此指令只能在http块中进行配置
# log_format  main '$remote_addr - $remote_user [$time_local] "$request" '
#                  '$status $body_bytes_sent "$http_referer" '
#                  '"$http_user_agent" "$http_x_forwarded_for"';
定义了上面的日志格式后，可以以下面的形式使用日志
# access_log  logs/access.log  main;

sendfile: linux 文件传输的一种方式
开启关闭sendfile方式传输文件，可以在http块、server块或者location块中进行配置
# sendfile  on | off;

# 设置sendfile最大数据量,此指令可以在http块、server块或location块中配置
# sendfile_max_chunk size;
# 其中，size值如果大于0，Nginx进程的每个worker process每次调用sendfile()传输的数据量最大不能超过这个值(这里是128k，所以每次不能超过128k)；如果设置为0，则无限制。默认值为0。
# sendfile_max_chunk 128k;

# 配置连接超时时间,此指令可以在http块、server块或location块中配置。
# 与用户建立会话连接后，Nginx服务器可以保持这些连接打开一段时间
# timeout，服务器端对连接的保持时间。默认值为75s;header_timeout，可选项，在应答报文头部的Keep-Alive域设置超时时间：“Keep-Alive:timeout= header_timeout”。报文中的这个指令可以被Mozilla或者Konqueror识别。
# keepalive_timeout timeout [header_timeout]
# 下面配置的含义是，在服务器端保持连接的时间设置为120 s，发给用户端的应答报文头部中Keep-Alive域的超时时间设置为100 s。
# keepalive_timeout 120s 100s

# 配置单连接请求数上限，此指令可以在http块、server块或location块中配置。
# Nginx服务器端和用户端建立会话连接后，用户端通过此连接发送请求。指令keepalive_requests用于限制用户通过某一连接向Nginx服务器发送请求的次数。默认是100
keepalive_requests number;

# 启用缓存时，NGINX将响应保存在磁盘缓存中，并使用它们来响应客户端，而不必每次都为同一内容代理请求。
# 强制的第一个参数是缓存内容的本地文件系统路径，强制keys_zone参数定义用于存储有关缓存项目的元数据的共享内存区域的名称和大小：
proxy_cache_path /data/nginx/cache keys_zone=name:10m;
然后在要缓存服务器响应的上下文(协议类型，虚拟服务器或位置)中包含proxy_cache指令，
由keys_zone参数定义的区域名称指定为proxy_cache_path指令
http {
    proxy_cache_path /data/nginx/cache keys_zone=one:10m;

    server {
        proxy_cache one;
        location / {
            proxy_pass http
        }
    }
}
```

##### server 块：虚拟主机
虚拟主机，又称虚拟服务器、主机空间或是网页空间。该技术是为了节省互联网服务器硬件成本而出现的。
虚拟主机 可以是服务器群或单个服务器。将多个服务器作为一个主机 或 将一个服务器作为多个主机
虚拟主机技术主要应用于HTTP、FTP及EMAIL等多项服务，将一台服务器的某项或者全部服务内容逻辑划分为多个服务单位，对外表现为多个服务器，从而充分利用服务器硬件资源。
从用户角度来看，一台虚拟主机和一台独立的硬件主机是完全一样的。
在使用Nginx服务器提供Web服务时，利用虚拟主机的技术就可以避免为每一个要运行的网站提供单独的Nginx服务器，也无需为每个网站对应运行一组Nginx进程。虚拟主机技术使得Nginx服务器可以在同一台服务器上只运行一组Nginx进程，就可以运行多个网站。
在前面提到过，每一个http块都可以包含多个server块，而每个server块就相当于一台虚拟主机，它内部可有多台主机联合提供服务，一起对外提供在逻辑上关系密切的一组服务（或网站）。
在server全局块中，最常见的两个配置项是本虚拟主机的监听配置和本虚拟主机的名称或IP配置。
```bash
root 指令定义服务器的默认网站根目录位置
root /var/www;

index 定义服务器主页文件名
index index.html；

server_name 定义虚拟主机的名称。语法是：
Syntax:    server_name name ...;
Default:    
server_name "";
Context:    server
对于name 来说，可以只有一个名称，也可以由多个名称并列，之间用空格隔开。每个名字就是一个域名，由两段或者三段组成，之间由点号“.”隔开。比如

server_name myserver.com www.myserver.com
在该例中，此虚拟主机的名称设置为myserver.com或www. myserver.com。Nginx服务器规定，第一个名称作为此虚拟主机的主要名称。

在name 中可以使用通配符“*”，但通配符只能用在由三段字符串组成的名称的首段或尾段，或者由两段字符串组成的名称的尾段，如：

server_name myserver.* *.myserver.com
另外name还支持正则表达式的形式。这边就不详细展开了。

由于server_name指令支持使用通配符和正则表达式两种配置名称的方式，因此在包含有多个虚拟主机的配置文件中，可能会出现一个名称被多个虚拟主机的server_name匹配成功。那么，来自这个名称的请求到底要交给哪个虚拟主机处理呢？Nginx服务器做出如下规定：

a. 对于匹配方式不同的，按照以下的优先级选择虚拟主机，排在前面的优先处理请求。

① 准确匹配server_name
② 通配符在开始时匹配server_name成功
③ 通配符在结尾时匹配server_name成功
④ 正则表达式匹配server_name成功
b. 在以上四种匹配方式中，如果server_name被处于同一优先级的匹配方式多次匹配成功，则首次匹配成功的虚拟主机处理请求。

有时候我们希望使用基于IP地址的虚拟主机配置，比如访问192.168.1.31有虚拟主机1处理，访问192.168.1.32由虚拟主机2处理。

这时我们要先网卡绑定别名，比如说网卡之前绑定的IP是192.168.1.30，现192.168.1.31和192.168.1.32这两个IP都绑定到这个网卡上，那么请求这个两个IP的请求才会到达这台机器。

绑定别名后进行以下配置即可：

http
{
    {
       listen:  80;       server_name:  192.168.1.31;
     ...
    }
    {
       listen:  80;
       server_name:  192.168.1.32;
     ...
    }
}

listen指令#
这个指令有三种配置语法,默认的配置值是：listen *:80 | *:8000；只能在server块种配置这个指令。

//第一种
listen address[:port] [default_server] [ssl] [http2 | spdy] [proxy_protocol] [setfib=number] [fastopen=number] [backlog=number] [rcvbuf=size] [sndbuf=size] [accept_filter=filter] [deferred] [bind] [ipv6only=on|off] [reuseport] [so_keepalive=on|off|[keepidle]:[keepintvl]:[keepcnt]];

//第二种
listen port [default_server] [ssl] [http2 | spdy] [proxy_protocol] [setfib=number] [fastopen=number] [backlog=number] [rcvbuf=size] [sndbuf=size] [accept_filter=filter] [deferred] [bind] [ipv6only=on|off] [reuseport] [so_keepalive=on|off|[keepidle]:[keepintvl]:[keepcnt]];

//第三种（可以不用重点关注）
listen unix:path [default_server] [ssl] [http2 | spdy] [proxy_protocol] [backlog=number] [rcvbuf=size] [sndbuf=size] [accept_filter=filter] [deferred] [bind] [so_keepalive=on|off|[keepidle]:[keepintvl]:[keepcnt]];
listen指令的配置非常灵活，可以单独制定ip，单独指定端口或者同时指定ip和端口。

listen 127.0.0.1:8000;  #只监听来自127.0.0.1这个IP，请求8000端口的请求
listen 127.0.0.1; #只监听来自127.0.0.1这个IP，请求80端口的请求（不指定端口，默认80）
listen 8000; #监听来自所有IP，请求8000端口的请求
listen *:8000; #和上面效果一样
listen localhost:8000; #和第一种效果一致
关于上面的一些重要参数做如下说明：

address：监听的IP地址（请求来源的IP地址），如果是IPv6的地址，需要使用中括号“[]”括起来，比如[fe80::1]等。
port：端口号，如果只定义了IP地址没有定义端口号，就使用80端口。这边需要做个说明：要是你压根没配置listen指令，那么那么如果nginx以超级用户权限运行，则使用*:80，否则使用*:8000。多个虚拟主机可以同时监听同一个端口,但是server_name需要设置成不一样；
default_server：假如通过Host没匹配到对应的虚拟主机，则通过这台虚拟主机处理。具体的可以参考这篇文章，写的不错。
backlog=number：设置监听函数listen()最多允许多少网络连接同时处于挂起状态，在FreeBSD中默认为-1，其他平台默认为511。
accept_filter=filter，设置监听端口对请求的过滤，被过滤的内容不能被接收和处理。本指令只在FreeBSD和NetBSD 5.0+平台下有效。filter可以设置为dataready或httpready，感兴趣的读者可以参阅Nginx的官方文档。
bind：标识符，使用独立的bind()处理此address:port；一般情况下，对于端口相同而IP地址不同的多个连接，Nginx服务器将只使用一个监听命令，并使用bind()处理端口相同的所有连接。
ssl：标识符，设置会话连接使用SSL模式进行，此标识符和Nginx服务器提供的HTTPS服务有关。
listen指令的使用看起来比较复杂，但其实在一般的使用过程中，相对来说比较简单，并不会进行太复杂的配置。
```

##### location
包含在 server 块中
```bash
root 定义访问文件的路径（到父级文件夹）
location /a.png {
    root /var/www/data/images   # 访问这个文件夹中的某个文件
}

return 跳转到一个错误页面, 第一个参数是响应代码。第二个参数可以是 重定向的URL 或 响应页面中的文本
return 404;  返回一个错误页面
return 404 错误;   第二个参数是错误页面显示的文本
return 404 www.aaa.com;  错误页面显示文本可以是一个网址

proxy_pass 定义一个代理页面：当访问 bbb 时,跳转由另一个服务器处理
location /bbb {
    proxy_pass http://www.bbb.com;
}

rewrite 在请求处理期间多次修改请求URI
第一个(必需)参数是请求URI必须匹配的正则表达式。 
第二个参数是用于替换匹配URI的URI。 
可选的第三个参数是可以停止进一步重写指令的处理或发送重定向(代码301或302)的标志
location /users/ {
    rewrite ^/users/(.*)$ /show?user=$1 break;
    rewrite ^/users/(.*)$ /show?user=$1 break;
}
可以写多个 rewrite，将一次寻找匹配的 URI，找到后根据新的 URI 跳转到新的 location
可以与 return 合用，如果全部不匹配，报错

有两个中断处理重写指令的参数：
last - 停止执行当前服务器或位置上下文中的 rewrite指令，
       但是NGINX会搜索与 rewrite 的URI匹配的位置，并且应用新位置中的任何 rewrite 指令(URI可以再次更改，往下继续匹配)。
break - 像break指令一样，在当前上下文中停止处理 rewrite指令，并取消搜索与新URI匹配的位置。
        新位置(location)块中的rewrite指令不执行。
```

##### location URI配对
```bash
当nginx决定哪个服务器处理请求后，它会根据服务器块内部定义的location指令的参数测试请求头中指定的URI 

当一个 URI 能够同时配被多 location 匹配的时候，则按顺序被第一个 location 所匹配。

以 server块 的 root 文件夹 为根目录，location匹配的URI 是 root 的相对路径 

这里假设我定义的 root 为 /usr/share/nginx/html/，访问的 URI 是 /hello/shiyanlou。
第一步：当 URI 被匹配后，会先查找 /usr/share/nginx/html/hello/shiyanlou 这个文件是否存在，
如果存在则返回，不存在进入下一步。
第二步：查找 /usr/share/nginx/html//hello/shiyanlou/ 目录是否存在，
如果存在，按 index 指定的文件名进行查找，比如 index.html，如果存在则返回，不存在就进入下一步。
第三步：返回 404 Not Found。

其语法如下：
location [ = | ~ | ~* | ^~ ] URI {
#    语句内容
}
其中各个符号的含义：

= 用于精准匹配，想请求的 URI 与 pattern 表达式完全匹配的时候才会执行 location 中的操作
~ 用于区分大小写的正则匹配；
~* 用于不区分大小写的正则匹配；
^~ 用于标准uri前，要求Nginx服务器找到标识uri和请求字符串匹配度最高的location后，
   立即使用此location处理请求，而不再使用location块中的正则uri和请求字符串做匹配。
我们以这样的实例来进一步理解：

location / {
    # [ 配置 A ]
}
当访问 www.root.com/index.html 时，请求将与配置 A 匹配；

location /documents/ {
    # [ 配置 B ]
}
当访问 www.root.com/documents/document.html 时，请求将匹配配置 B;

location = / {
    # [ 配置 C ]
}
当访问 www.root.com 时，请求访问的是 /，所以将与配置 C 匹配；

location ^~ /images/ {
    # [ 配置 D ]
}
当访问 www.root.com/images/1.gif 请求将匹配配置 D；

location ~* \.(gif|jpg|jpeg)$ {
    # [ 配置 E ]
}
当访问 www.root.com/docs/1.jpg 请求将匹配配置 E。
```

##### 基本配置：configure命令
```bash
使用configure命令配置构建。 它定义了系统的各个方面，包括允许使用nginx进行连接处理的方法。 最后它创建一个Makefile。 
./configure --sbin-path=/usr/local/nginx/nginx
configure命令支持以下参数：
--prefix = path - 定义将保留服务器文件的目录。 这个同一个目录也将用于由configure(除了库源的路径)和nginx.conf配置文件中设置的所有相关路径。 它默认设置为/usr/local/nginx目录。
--prefix=/usr/share/nginx
--sbin-path = path - 设置nginx可执行文件的名称。此名称仅在安装期间使用。默认情况下文件名为 prefix/sbin/nginx。

--conf-path = path - 设置nginx.conf配置文件的名称。 如果需要，nginx可以始终使用不同的配置文件启动，方法是在命令行参数-c file 指定。 默认情况下，该文件名为：prefix/conf/nginx.conf。
 --conf-path=/etc/nginx/nginx.conf
--pid-path = path - 设置将存储主进程的进程ID的nginx.pid文件的名称。 安装后，可以使用pid指令在nginx.conf配置文件中更改文件名。 默认情况下，文件名为：prefix/logs/nginx.pid。
--pid-path=/run/nginx.pid
--error-log-path = path - 设置主错误，警告和诊断文件的名称。 安装后，可以在nginx.conf配置文件中使用error_log指令更改文件名。 默认情况下，文件名为：prefix/logs/error.log。
 --error-log-path=/var/log/nginx/error.log
--http-log-path = path - 设置HTTP服务器主要请求日志文件的名称。 安装后，可以使用access_log指令在nginx.conf配置文件中更改文件名。 默认情况下，文件名为：prefix/logs/access.log。
--http-log-path=/var/log/nginx/access.log 
--build = name - 设置一个可选的nginx构建名称。

--user = name - 设置非特权用户的名称，该用户的凭据将由工作进程使用。 安装后，可以使用user指令在nginx.conf配置文件中更改名称。 默认的用户名是：nobody。
--group = name - 设置由工作进程使用其凭据的组的名称。 安装后，可以使用user指令在nginx.conf配置文件中更改名称。 默认情况下，组名称设置为非特权用户的名称。

--with-select_module 和 --without-select_module — 启用或禁用构建允许服务器使用select()方法的模块。 如果平台似乎不支持更合适的方法(如kqueue，epoll或/dev/poll)，则会自动构建该模块。
--with-poll_module 和 --without-poll_module — 启用或禁用构建允许服务器使用poll()方法的模块。 如果平台似乎不支持更合适的方法(如kqueue，epoll或/dev/poll)，则会自动构建该模块。

--without-http_gzip_module - 禁用构建压缩HTTP服务器响应的模块。 需要zlib库来构建和运行此模块。
 --with-http_gunzip_module
--without-http_rewrite_module - 禁用构建一个允许HTTP服务器重定向请求并更改请求URI的模块。 需要PCRE库来构建和运行此模块。

--without-http_proxy_module - 禁用构建HTTP服务器代理模块。
--with-http_ssl_module - 可以构建一个将HTTPS协议支持添加到HTTP服务器的模块。 默认情况下不构建此模块。 OpenSSL库是构建和运行该模块所必需的。
--with-http_ssl_module
--with-pcre = path - 设置PCRE库源的路径。库发行版(4.4 - 8.40版)需要从PCRE站点下载并提取。 其余的由nginx的./configure和make完成。 该库是 location 指令和ngx_http_rewrite_module模块中正则表达式支持所必需的。

--with-pcre-jit - 使用“即时编译”支持构建PCRE库。
--with-pcre-jit
--with-zlib = path - 设置zlib库的源路径。 库分发(版本1.1.3 - 1.2.11)需要从zlib站点下载并提取。 其余的由nginx的./configure和make完成。 该库是ngx_http_gzip_module模块所必需的。

--with-cc-opt = parameters - 设置将添加到CFLAGS变量的其他参数。 在FreeBSD下使用系统PCRE库时，应指定--with-cc-opt="-I /usr/local/include"。 如果需要增加select()所支持的文件数，那么也可以在这里指定，如：--with-cc-opt="-D FD_SETSIZE=2048"。
--with-ld-opt = parameters - 设置链接过程中使用的其他参数。 当在FreeBSD下使用系统PCRE库时，应指定--with-ld-opt="-L /usr/local/lib"。
```

### 在 Ubuntu环境 下将一个 vue 项目部署 nginx 服务器
1.将项目打包成 dist 包
2.将 dist 包复制到服务器
```bash
ubuntu 系统下有效：

本地文件上传到服务器
scp -r localfile.txt username@192.168.0.1:/home/username/ 
其中， 
１）scp是命令，-r是参数 
２）localfile.txt 是文件的路径和文件名 （本地）
３）username是服务器账号 
４）192.168.0.1是要上传的服务器ip地址 
５）/home/username/是要拷入的文件夹路径（服务器路径）

scp root@192.168.1.155:1.txt 2.txt （把服务器的1.txt下载到本地，并且重命名为2.txt）
scp 2.txt root@192.168.1.155:3.txt （把本地2.txt文件上传到服务器的root目录下，并且命名为3.txt）
```
3.修改 nginx配置文件
```bash
# 修改nginx.conf配置文件
sudo vim /etc/nginx/nginx.conf
# 将 server块内的 root改为 dist包的路径

sudo nginx -t # 检查文本错误
sudo nignx -s reload # 应用修改
```

### 错误收集
第一次部署网页之后，发现依然显示默认的 nginx 欢迎页
```bash
解决方法：注释掉配置文件中下面两行，重启配置文件
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```




