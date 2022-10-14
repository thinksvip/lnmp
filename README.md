# **LNMP**（Docker + Nginx + MySQL + PHP + Redis）**PHP统一开发环境**。


## 1.项目结构
目录说明：
```
    \
    ├── conf                                配置文件目录
    │   ├── mysql                           
    │   │   ├── my5.7.cnf                   mysql5.7配置文件
    │   │   └── my8.0.cnf                   mysql8.0配置文件
    │   ├── nginx                           
    │   │   ├── conf.d                      nginx用户站点配置目录
    │   │   │   ├── certs                   ssl证书配置目录
    │   │   │   ├── site1.conf              默认演示站点
    │   │   │   ├── site2.conf              https默认演示站点
    │   │   │   └── upstream.conf           fastcgi_pass配置文件
    │   │   └── nginx.conf                  nginx默认配置文件
    │   ├── php                             多版本php配置
    │   │   ├── php                         
    │   │   │   ├── php-fpm.conf            php-fpm默认配置文件
    │   │   │   ├── php.ini                 php默认配置
    │   │   │   └── zz-docker.conf          fastcgi_pass 监听配置文件 unix socket / tcp socket
    │   │   └── php8
    │   │       ├── php-fpm.conf
    │   │       ├── php.ini
    │   │       └── zz-docker.conf
    │   └── redis           
    │       └── redis.conf                  redis默认配置
    ├── log                                 日志文件
    │   ├── mysql
    │   ├── nginx
    │   ├── php-fpm
    │   └── redis
    ├── mysql                               
    │   └── Dockerfile                      mysql镜像构建文件
    ├── nginx                               
    │   └── Dockerfile                      nginx镜像构建文件
    ├── node                    
    │   ├── Dockerfile                      node镜像构建文件
    │   ├── laravel-echo-server
    │   │   └── laravel-echo-server.json    支持 laravel echo server
    │   └── package.json
    ├── php
    │   ├── php
    │   │   └── Dockerfile                  php镜像构建文件
    │   ├── php8
    │   │   └── Dockerfile                  php8镜像构建文件
    │   ├── php-queue
    │   │   └── Dockerfile                  php-queue镜像构建文件
    │   └── php-xdebug
    │       └── Dockerfile                  php镜像构建文件 含xdebug 开发环境使用
    ├── redis
    │   └── Dockerfile                      redis镜像构建文件
    ├── www                                 项目源码目录
    │   └── site                            默认演示文件
    │       └── index.php
    ├── data                                数据存放目录
    ├── docker-compose-dev.yml              开发环境
    ├── docker-compose.yml                  生成环境
    ├── env-example                         环境变量文件
    └── README.md                           说明文档
```



## 2. 快速使用
1. 本地安装`git`、`docker`和`docker-compose`。
2. `clone`项目：

```
    $ git clone https://github.com/thinksvip/lnmp.git
```

3. 如果不是`root`用户，还需将当前用户加入`docker`用户组：

```
    $ sudo gpasswd -a ${USER} docker
```

4. 启动：

```
    $ cd lnmp
    $ cp env-example .env
    $ docker-compose up
```

5. 访问在浏览器中访问：

 - [http://localhost](http://localhost)： 默认*http*站点
 - [https://localhost](https://localhost)：自定义证书*https*站点，访问时浏览器会有安全提示，忽略提示访问即可

两个站点使用同一PHP代码：`./www/site/index.php`。


## 3. 切换PHP版本？
默认情况下，我们启用php72版本的容器，

切换PHP版本仅需修改.env 配置的`PHP_FPM_VERSION`选项，

例如，示例的**localhost**用的是PHP7.2，.env 配置：
```
    PHP_FPM_VERSION=7.2
```
要改用PHP5.6，修改为：
```
    PHP_FPM_VERSION=5.6
```
再 **终止LNMP 重新启动 LNMP** 生效。
```
    $ docker-compose down
    $ docker-compose up 
```
新增多版本PHP集成
```
    $ docker-compose up php php8
```
<span style="color:red;">注意：PHP多版本使用Nginx fastcgi_pass 配置如下</span>


PHP7 及以下
```
    location ~ \.php$ {
        fastcgi_pass   php:9000;
        fastcgi_index  index.php;
        include        fastcgi_params;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    }
```

PHP8
```
    location ~ \.php$ {
        fastcgi_pass   php8:9000;
        fastcgi_index  index.php;
        include        fastcgi_params;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    }
```

## 4. 添加快捷命令
在开发的时候，我们可能经常使用docker exec -it切换到容器中，把常用的做成命令别名是个省事的方法。

打开~/.bashrc，加上：
```bash
    alias dnginx='docker exec -it nginx /bin/sh'
    alias dphp='docker exec -it php sh'
    alias dmysql='docker exec -it mysql /bin/bash'
    alias dredis='docker exec -it redis /bin/bash'
    alias dqueue='docker exec -it queue sh'
    alias dnode='docker exec -it node sh'       
```

## 5. 使用Log

Log文件生成的位置依赖于conf下各log配置的值。

### 5.1 Nginx日志
Nginx日志是我们用得最多的日志，所以我们单独放在根目录`log`下。

`log`会目录映射Nginx容器的`/var/log/nginx`目录，所以在Nginx配置文件中，需要输出log的位置，我们需要配置到`/var/log/nginx`目录，如：
```
    error_log  /var/log/nginx/nginx.localhost.error.log  warn;
```


### 5.1 PHP-FPM日志
大部分情况下，PHP-FPM的日志都会输出到Nginx的日志中，所以不需要额外配置。

如果确实需要，可按一下步骤开启。

1. 在主机中创建日志文件并修改权限：

```bash
    $ touch log/php-fpm/php-fpm.error.log
    $ chmod a+w log/php-fpm.error.log
```
2. 主机上打开并修改PHP-FPM的配置文件`conf/www.conf`，找到如下一行，删除注释，并改值为：

```
    php_admin_value[error_log] = /var/log/php-fpm/php-fpm.error.log
```
3. 重启PHP-FPM容器。

### 5.2 MySQL日志
因为MySQL容器中的MySQL使用的是`mysql`用户启动，它无法自行在`/var/log`下的增加日志文件。在启动容器组后给`/var/log/mysql/`文件夹更所有者为`mysql`。更改文件夹所有者后重启`mysql`容器。
```
    $ docker exec mysql chown mysql:root /var/log/mysql
    $ docker-compose restart mysql
```
重启完成后在`./log/mysql/`会生成`mysql-slow.log`日志文件。
## 6. 使用composer
lnmp默认已经在容器中安装了composer，使用时先进入容器：
```
    $ docker exec -it php sh
```
然后进入相应目录，使用composer：
```
    $ cd /var/www/html/
    $ composer install
```
因为composer依赖于PHP，所以，是必须在容器里面操作composer的。

## 7. 使用opcache
**注意**opcache已开启,配置默认不查询 **更改** 如发布新版本需重启php-fpm
```
    $ docker-compose restart php
``` 

如需关闭 修改`./conf/php/php.ini` 中 `opcache.enable=1`的值为`0`
```
    opcache.enable=0
```
`opcache`具体配置需查看`./conf/php/php.ini`中 `[opcache]`
## 8. 使用queue
设置项目路径
```
    $ sed -i 's/APP_CODE_PATH=/APP_CODE_PATH=Your project name/g' .env

```
查看调度输出
```
    $ docker-compose logs queue
```

## 9. 使用xdebug
默认情况下，我们已经`dev`环境安装了`Xdebug`的扩展，但并未在`php.ini`文件中配置启用。要使用Xdebug的调试，在`./conf/php/php.ini`的文件最后这几行将注释取消：
```
    [XDebug]
    xdebug.enable = on
    xdebug.remote_enable = on
    xdebug.remote_connect_back = on
    xdebug.idekey = "PHPSTORM"
    xdebug.remote_autostart = off
    xdebug.remote_handler = "dbgp"
    xdebug.remote_host = docker.for.win.localhost #如果你是 Mac 用户，修改为 docker.for.mac.localhost
    xdebug.remote_port = "9001"
    xdebug.remote_log = "/var/log/php-fpm/xdebug_remote.log"
```
然后重启PHP容器。
```
    $ docker-compose restart php
```
## 10. linux 部署需开启NAT转发

### centos8.2 firewall防火墙为例

```
    firewall-cmd --permanent --zone=public --add-masquerade
    firewall-cmd --reload
    systemctl restart firewalld 
    systemctl restart docker
```

## 11. 在正式环境中安全使用
要在正式环境中使用，请：
1. 在php.ini中关闭XDebug调试
2. 增强MySQL数据库访问的安全策略
3. 增强redis访问的安全策略

## 常见问题
1. 遇到“No releases available for package "pecl.php.net/redis”
    > 请参考： https://github.com/yeszao/dnmp/issues/10

说明：**这个问题主要是受国内网络环境影响，现在PHP7以上的版本直接采用从源码安装扩展，所以这个问题已经没有了。**

2. PHP5.6错误“ibfreetype6-dev : Depends: zlib1g-dev but it is not going to be installed or libz-dev”
    > 请参考： https://github.com/yeszao/dnmp/issues/39
3. `mysql`日志文件授权为何不在`Dockerfile`中使用`RUN` 命令直接授权。因为`Dockerfile`中`VOLUME`指令之后的任何内容都无法对该卷进行更改。我们的镜像是基于官方镜像搭建，官方镜像在构建时使用过`VOLUME`指令。
   >请参考： https://container-solutions.com/understanding-volumes-docker/
4. 项目权限问题 
   >进入php容器 `docker-compose exec php sh`

   >授权 `chown -R www-data:www-data /var/www/html`

## 搭建参考
1. > https://github.com/yeszao/dnmp
2. > http://laradock.io/

## License
MIT


