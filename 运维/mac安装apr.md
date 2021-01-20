## mac安装apr

#### 1.安装步骤

* 安装编译安装apr-*
* 编译安装apr-util-1.6.0.tar.gz
* tomcat-native(默认的openssl版本不匹配的需要重新安装openssl)

具体步骤可以参考:https://blog.csdn.net/u014497502/article/details/51503151

#### 2.安装openssl

```shell
#root操作
./Configure darwin64-x86_64-cc --prefix=/usr/local/opt/openssl no-asm --openssldir=/usr/local/ssl
#编译
make
#安装
make install

#当编译报错使用make clean清理在编译

```



具体参考:
https://blog.csdn.net/ccgshigao/article/details/108354707