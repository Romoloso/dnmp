### DNMP��Docker + Nginx + MySQL + PHP7��
�汾��     
- nginx: alpine ����        
- MySQL: 8.0      
- PHP: 7.3        

�ص㣺
1. ��ѭDocker��׼    
2. ֧�ְ�����������
3. �������ýԿ��޸�
4. ��־�ļ��Կɲ鿴
5. ��������php��չ��������չ�����汾
6. windows,linux ��������

#### php ��չ
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

#### Ŀ¼�ṹ
```
������ conf
��   ������ mysql
��   ��   ������ mysql.cnf
��   ������ nginx
��   ��   ������ conf.d
��   ��   ��   ������ default.conf
��   ��   ������ nginx.conf
��   ������ php
��       ������ php-fpm.d
��       ��   ������ www.conf
��       ������ php.ini
������ docker-compose.yml
������ log
��   ������ mysql
��   ������ nginx
��   ������ php-fpm
������ mysql
������ php
��   ������php73
��      ������ composer.phar
��      ������ Dockerfile
��      ������ memcached-3.1.4.tgz
��      ������ mongodb-1.6.0.tgz
��      ������ redis-5.0.0.tgz
��      ������ sources.list.stretch
��      ������ swoole-4.4.12.tgz
��      ������ xdebug-2.8.0.tgz
��      ������ xhprof-2.1.0.tar.gz
��   
��   
������ www

```