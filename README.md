### DNMP（Docker + Nginx + MySQL + PHP7）
版本：     
- nginx: alpine 最新        
- MySQL: 8.0      
- PHP: 7.3        

特点：
1. 遵循Docker标准    
2. 支持绑定任意多个域名
3. 所有配置皆可修改
4. 日志文件皆可查看
5. 内置完整php扩展，并可扩展其他版本
6. windows,linux 都可以用

#### php 扩展
```
[PHP Modules]
bcmath
calendar
Core
ctype
curl
date
dom
exif
fileinfo
filter
ftp
gd
gettext
hash
iconv
json
libxml
mbstring
memcached
mongodb
msgpack
mysqli
mysqlnd
openssl
pcntl
pcre
PDO
pdo_mysql
pdo_pgsql
pdo_sqlite
Phar
posix
readline
redis
Reflection
session
shmop
SimpleXML
soap
sockets
sodium
SPL
sqlite3
standard
swoole
sysvmsg
sysvsem
sysvshm
tokenizer
wddx
xdebug
xhprof
xml
xmlreader
xmlrpc
xmlwriter
xsl
Zend OPcache
zip
zlib

[Zend Modules]
Xdebug
Zend OPcache

```

#### 目录结构
```
├── conf
│   ├── mysql
│   │   └── mysql.cnf
│   ├── nginx
│   │   ├── conf.d
│   │   │   └── default.conf
│   │   └── nginx.conf
│   └── php
│       ├── php-fpm.d
│       │   └── www.conf
│       └── php.ini
├── docker-compose.yml
├── log
│   ├── mysql
│   ├── nginx
│   └── php-fpm
├── mysql
├── php
│   └──php73
│      ├── composer.phar
│      ├── Dockerfile
│      ├── memcached-3.1.4.tgz
│      ├── mongodb-1.6.0.tgz
│      ├── redis-5.0.0.tgz
│      ├── sources.list.stretch
│      ├── swoole-4.4.12.tgz
│      ├── xdebug-2.8.0.tgz
│      └── xhprof-2.1.0.tar.gz
│   
│   
└── www

```