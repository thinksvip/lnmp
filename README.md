# **LNMP**（Docker + Nginx + MySQL + PHP7/5 + Redis）**PHP统一开发环境**。


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
    │   ├── php
    │   │   ├── php-fpm.d                   php配置目录
    │   │   │   ├── www.conf                php-fpm默认配置文件
    │   │   │   └── zz-docker.conf          fastcgi_pass 监听配置文件 unix socket / tcp socket
    │   │   └── php.ini                     php默认配置
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
    ├── php
    │   ├── composer.phar                   composer安装包1.7.2
    │   ├── Dockerfile                      php镜像构建文件
    │   └── sources.list.stretch            Debian源目录
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
    $ git clone git@github.com:thinksvip/lnmp.git
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

## 4. 添加快捷命令
在开发的时候，我们可能经常使用docker exec -it切换到容器中，把常用的做成命令别名是个省事的方法。

打开~/.bashrc，加上：
```bash
    alias dnginx='docker exec -it nginx /bin/sh'
    alias dphp='docker exec -it php /bin/bash'
    alias dmysql='docker exec -it mysql /bin/bash'
    alias dredis='docker exec -it redis /bin/bash'
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
    touch log/php-fpm/php-fpm.error.log
    chmod a+w log/php-fpm.error.log
```
2. 主机上打开并修改PHP-FPM的配置文件`conf/www.conf`，找到如下一行，删除注释，并改值为：
```
    php_admin_value[error_log] = /var/log/php-fpm/php-fpm.error.log
```
3. 重启PHP-FPM容器。

### 5.2 MySQL日志
因为MySQL容器中的MySQL使用的是`mysql`用户启动，它无法自行在`/var/log`下的增加日志文件。所以在创建`mysql`构建镜像`./mysql/Dockerfile`时候，我们将`/var/log/mysql/`文件夹授权为`mysql`用户/组所有。
```
    chown -R mysql:mysql /var/log/mysql/
```
以上是mysql.conf中的日志文件的配置。

## 6. 使用composer
lnmp默认已经在容器中安装了composer，使用时先进入容器：
```
    $ docker exec -it php /bin/bash
```
然后进入相应目录，使用composer：
```
    $ cd /var/www/html/
    $ composer update
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

## 8. 使用xdebug
默认情况下，我们已经安装了`Xdebug`的扩展，但并未在`php.ini`文件中配置启用。要使用Xdebug的调试，在`./conf/php/php.ini`的文件最后这几行将注释取消：
```
    [XDebug]
    xdebug.enable = on
    xdebug.remote_enable = on
    xdebug.remote_connect_back = on
    xdebug.idekey = "PHPSTORM"
    xdebug.remote_autostart = on
    xdebug.remote_handler = "dbgp"
    xdebug.remote_host = "192.168.31.210"
    xdebug.remote_port = "9001"
    xdebug.remote_log = "/var/log/php-fpm/xdebug_remote.log"
```
然后重启PHP容器。
```
    $ docker-compose restart php
```

## 9. 在正式环境中安全使用
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

## 搭建参考
1. > https://github.com/yeszao/dnmp
2. > http://laradock.io/

## License
MIT


