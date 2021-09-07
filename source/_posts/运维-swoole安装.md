---
title: 运维-swoole安装
date: '2020-09-04 18:24:44 - 运维 - 服务器 - swoole'
categories:
  - IT
  - 运维
abbrlink: 7a259a26
---

```
https://github.com/nghttp2/nghttp2


yum install libev-devel libevent-devel c-ares-devel jemalloc-devel jansson-devel python-devel zlib-devel

./configure
make
make install

echo '/usr/local/lib' > /etc/ld.so.conf.d/local.conf
ldconfig


yum install hiredis-devel


pecl install https://pecl.php.net/get/swoole-2.2.0.tgz



### 异常处理
yum install centos-release-scl
yum install devtoolset-7
scl enable devtoolset-7 bash



apt-get install libwebp-dev libjpeg-dev libpng-dev libxpm-dev libfreetype6-dev libvpx-dev


apt-get install libpcap-dev libnet-dev libnids-dev libedit-dev libsodium-dev


sudo update-alternatives --config php

cd /opt/php-7.3.1

sudo ./configure --prefix=/usr/local/php7.3.1 --enable-bcmath --enable-exif --enable-fpm --enable-intl --enable-mbstring --enable-mysqlnd --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-zip --with-curl --with-gd --with-mysql-sock --with-openssl --with-pdo-mysql --with-zlib

sudo ./configure --enable-bcmath --enable-exif --enable-fpm --enable-intl --enable-mbstring --enable-mysqlnd --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-zip --with-curl --with-gd --with-mysql-sock --with-openssl --with-pdo-mysql --with-zlib

make clean
make
make install


cd /opt/php-7.3.1/ext/zlib
mv config0.4 config.m4
./configure
make
make install


cd /opt/swoole-2.2.0/
sudo ./configure  --enable-sockets --enable-async-redis --enable-openssl --enable-http2 --enable-mysqlnd
```

```
  --configure-options [--includedir=/usr/include --mandir=/usr/share/man --infodir=/usr/share/info --disable-silent-rules --libdir=/usr/lib/x86_64-linux-gnu --libexecdir=/usr/lib/x86_64-linux-gnu --disable-maintainer-mode --disable-dependency-tracking --prefix=/usr --enable-cli --disable-cgi --disable-phpdbg --with-config-file-path=/etc/php/7.3/cli --with-config-file-scan-dir=/etc/php/7.3/cli/conf.d --build=x86_64-linux-gnu --host=x86_64-linux-gnu --config-cache --cache-file=/build/php7.3-R8bPLl/php7.3-7.3.21/config.cache --libdir=${prefix}/lib/php --libexecdir=${prefix}/lib/php --datadir=${prefix}/share/php/7.3 --program-suffix=7.3 --sysconfdir=/etc --localstatedir=/var --mandir=/usr/share/man --disable-all --disable-debug --disable-rpath --disable-static --with-pic --with-layout=GNU --without-pear --enable-filter --with-openssl=yes --with-password-argon2=/usr --with-pcre-regex=/usr --enable-hash --with-mhash=/usr --enable-libxml --enable-session --with-sodium --with-system-tzdata --with-zlib=/usr --with-zlib-dir=/usr --enable-dtrace --enable-pcntl --with-libedit=shared,/usr build_alias=x86_64-linux-gnu host_alias=x86_64-linux-gnu CFLAGS=-g -O2 -fdebug-prefix-map=/build/php7.3-R8bPLl/php7.3-7.3.21=. -fstack-protector-strong -Wformat -Werror=format-security -O2 -Wall -pedantic -fsigned-char -fno-strict-aliasing -g]


  --includedir=/usr/include --mandir=/usr/share/man --infodir=/usr/share/info --disable-silent-rules --libdir=/usr/lib/x86_64-linux-gnu --libexecdir=/usr/lib/x86_64-linux-gnu --disable-maintainer-mode --disable-dependency-tracking --prefix=/usr --enable-cli --disable-cgi --disable-phpdbg --with-config-file-path=/etc/php/7.3/cli --with-config-file-scan-dir=/etc/php/7.3/cli/conf.d --build=x86_64-linux-gnu --host=x86_64-linux-gnu --config-cache --cache-file=/build/php7.3-R8bPLl/php7.3-7.3.21/config.cache --libdir=${prefix}/lib/php --libexecdir=${prefix}/lib/php --datadir=${prefix}/share/php/7.3 --program-suffix=7.3 --sysconfdir=/etc --localstatedir=/var --mandir=/usr/share/man --disable-all --disable-debug --disable-rpath --disable-static --with-pic --with-layout=GNU --without-pear --enable-filter --with-openssl=yes --with-password-argon2=/usr --with-pcre-regex=/usr --enable-hash --with-mhash=/usr --enable-libxml --enable-session --with-sodium --with-system-tzdata --with-zlib=/usr --with-zlib-dir=/usr --enable-pcntl --with-libedit=shared
```

```
/bin/bash /opt/php-7.3.1/libtool --silent --preserve-dup-deps --mode=install cp ext/opcache/opcache.la /opt/php-7.3.1/modules
Installing shared extensions:     /usr/local/php7.3.1/lib/php/extensions/no-debug-non-zts-20180731/
Installing PHP CLI binary:        /usr/local/php7.3.1/bin/
Installing PHP CLI man page:      /usr/local/php7.3.1/php/man/man1/
Installing PHP FPM binary:        /usr/local/php7.3.1/sbin/
Installing PHP FPM defconfig:     /usr/local/php7.3.1/etc/
Installing PHP FPM man page:      /usr/local/php7.3.1/php/man/man8/
Installing PHP FPM status page:   /usr/local/php7.3.1/php/php/fpm/
Installing phpdbg binary:         /usr/local/php7.3.1/bin/
Installing phpdbg man page:       /usr/local/php7.3.1/php/man/man1/
Installing PHP CGI binary:        /usr/local/php7.3.1/bin/
Installing PHP CGI man page:      /usr/local/php7.3.1/php/man/man1/
Installing build environment:     /usr/local/php7.3.1/lib/php/build/
Installing header files:          /usr/local/php7.3.1/include/php/
Installing helper programs:       /usr/local/php7.3.1/bin/
  program: phpize
  program: php-config
Installing man pages:             /usr/local/php7.3.1/php/man/man1/
  page: phpize.1
  page: php-config.1
Installing PEAR environment:      /usr/local/php7.3.1/lib/php/
[PEAR] Archive_Tar    - already installed: 1.4.4
[PEAR] Console_Getopt - already installed: 1.4.1
[PEAR] Structures_Graph- already installed: 1.1.1
[PEAR] XML_Util       - already installed: 1.4.3
[PEAR] PEAR           - already installed: 1.10.7
Warning! a PEAR user config file already exists from a previous PEAR installation at '/home/vagrant/.pearrc'. You may probably want to remove it.
Wrote PEAR system config file at: /usr/local/php7.3.1/etc/pear.conf
You may want to add: /usr/local/php7.3.1/lib/php to your php.ini include_path
/opt/php-7.3.1/build/shtool install -c ext/phar/phar.phar /usr/local/php7.3.1/bin
ln -s -f phar.phar /usr/local/php7.3.1/bin/phar
Installing PDO headers:           /usr/local/php7.3.1/include/php/ext/pdo/
```


```
sudo ./configure --enable-fpm --enable-bcmath --enable-exif --enable-intl --enable-mbstring --enable-embedded-mysqli --enable-sockets --enable-soap --enable-mysqlnd --enable-zip --with-openssl --with-curl --with-gd --with-pdo-mysql --with-mysql-sock --enable-cli --disable-cgi --disable-phpdbg --build=x86_64-linux-gnu --host=x86_64-linux-gnu --config-cache --program-suffix=7.3 --disable-all --disable-debug --disable-rpath --disable-static --with-pic --with-layout=GNU --without-pear --enable-filter --with-openssl=yes --enable-hash --enable-libxml --enable-session --with-sodium --enable-pcntl --with-libedit=shared --enable-pdo
```


```
sudo ./configure --includedir=/usr/include --mandir=/usr/share/man --infodir=/usr/share/info --libdir=/usr/lib/x86_64-linux-gnu --libexecdir=/usr/lib/x86_64-linux-gnu  --prefix=/usr --enable-cli --disable-cgi --disable-phpdbg --with-config-file-path=/etc/php/7.3/cli --with-config-file-scan-dir=/etc/php/7.3/cli/conf.d --build=x86_64-linux-gnu --host=x86_64-linux-gnu --config-cache --cache-file=/build/php7.3-R8bPLl/php7.3-7.3.21/config.cache --libdir=${prefix}/lib/php --libexecdir=${prefix}/lib/php --datadir=${prefix}/share/php/7.3 --program-suffix=7.3 --sysconfdir=/etc --localstatedir=/var --mandir=/usr/share/man --disable-all --disable-debug --disable-rpath --disable-static --with-pic --with-layout=GNU --without-pear --enable-filter --with-openssl=yes  --with-pcre-regex=/usr --enable-hash --with-mhash=/usr --enable-libxml --enable-session --with-sodium --with-zlib=/usr --with-zlib-dir=/usr  --enable-pcntl
```


```
Installing shared extensions:     /usr/local/lib/php/extensions/no-debug-non-zts-20180731/
Installing PHP CLI binary:        /usr/local/bin/
Installing PHP CLI man page:      /usr/local/php/man/man1/
Installing PHP FPM binary:        /usr/local/sbin/
Installing PHP FPM defconfig:     /usr/local/etc/
Installing PHP FPM man page:      /usr/local/php/man/man8/
Installing PHP FPM status page:   /usr/local/php/php/fpm/
Installing phpdbg binary:         /usr/local/bin/
Installing phpdbg man page:       /usr/local/php/man/man1/
Installing PHP CGI binary:        /usr/local/bin/
Installing PHP CGI man page:      /usr/local/php/man/man1/
Installing build environment:     /usr/local/lib/php/build/
Installing header files:          /usr/local/include/php/
Installing helper programs:       /usr/local/bin/
  program: phpize
  program: php-config
Installing man pages:             /usr/local/php/man/man1/
  page: phpize.1
  page: php-config.1
Installing PEAR environment:      /usr/local/lib/php/
[PEAR] Archive_Tar    - installed: 1.4.4
[PEAR] Console_Getopt - installed: 1.4.1
[PEAR] Structures_Graph- installed: 1.1.1
[PEAR] XML_Util       - installed: 1.4.3
[PEAR] PEAR           - installed: 1.10.7
Warning! a PEAR user config file already exists from a previous PEAR installation at '/home/vagrant/.pearrc'. You may probably want to remove it.
Wrote PEAR system config file at: /usr/local/etc/pear.conf
You may want to add: /usr/local/lib/php to your php.ini include_path
/opt/php-7.3.1/build/shtool install -c ext/phar/phar.phar /usr/local/bin
ln -s -f phar.phar /usr/local/bin/phar
Installing PDO headers:           /usr/local/include/php/ext/pdo/
```