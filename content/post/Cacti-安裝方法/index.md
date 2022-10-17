+++
author = "Hugo Authors"
title = "Cacti-安裝方法"
date = "2022-08-07"
description = "(包含前置作業LAMP)"
categories = [
    "Cacti"
]
tags = [
    "Cacti",
]
image = "100.jpg"
+++

    #LAMP版本相依
    mysql版本：5.1.73或5.6
    apache版本：2.2.27
    php版本：5.5.16
    
    #安裝順序mysql > apache > PHP
    
    MySQL :
    #查看有無舊版本，若有則用rpm-e --nodeps強制刪除
    rpm -qa | grep mariadb
    rpm -qa | grep mysql
    #安裝mysql
    yum install mysql-community-server mysql-devel (centOS6 預設版本mysqk為5.1.73)
    
    Apache編譯語法 :
    ./configure --prefix=/usr/local/web/apache2 --enable-ssl --enable-so  --enable-vhost-alias  --with-mpm=prefork
    
    PHP編譯語法 :
    './configure' '--prefix=/usr/local/web/php' '--with-apxs2=/usr/local/web/apache2/bin/apxs' '--with-mysql' '--with-mysqli' '--disable-cgi' '--with-iconv' '--disable-inline-optimization' '--enable-mbstring=tw' '--enable-sysvshm' '--enable-sysvsem' '--enable-sockets' '--with-jpeg-dir' '--with-png-dir' '--with-gd' '--with-zlib' '--with-curl' '--enable-zip' '--with-openssl-dir=/usr/lib/openssl' '--with-openssl' '--enable-opcache'
    
    #安裝cacti
    tar zxvf cacti-0.8.7g.tar.gz
    
    再將cacti放置web存取的目錄下
    mv cacti-0.8.7g /home/cacti
    
    #安裝snmp
    yum install -y net-snmp  net-snmp-utils
    
    #安裝RRDTool
    cd rrdtool-1.0.50
    ./configure --prefix=/usr/local/rrdtool 
    make
    make install
    
    #如果編譯有問題 可嘗試下面兩行指令 可能缺少的套件
    #ln -s /usr/local/libpng/lib/libpng.so /usr/lib/ 
    #ln -s /usr/local/freetype/lib/libfreetype.so /usr/lib/
    #yum -y install freetype-devel
    #yum -y install libpng-devel
    #yum -y install libart_lgpl-devel
    
    啟動mysql
    mysqld_safe &
 
    建立mysql的root帳號與密碼   
    mysqladmin -u root password '密碼' 
    
    建立資料庫 cacti
    mysqladmin --user=root -p create cacti
    
    將 cacti 目錄底下的cacti.sql 匯入資料庫 cacti 
    cd    /home/cacti/ 
    mysql -u root -p cacti < cacti.sql 
    
    進入MySQL並建立一個存取cacti資料庫的使用者cactiuser與密碼123456
    並給予權限 
    mysql    -u    root    -p 
    mysql> grant all on cacti.* to cactiuser@localhost identified by '123456'; 
    mysql> flush privileges; 
    mysql> exit 
    
    vim /home/cacti/include/config.php 
    (config.php 定義在cacti 資料庫進行存取時，所使用的帳號及密碼) 
    
      26 $database_type = "mysql"; 
      27 $database_default = "cacti"; 
      28 $database_hostname = "localhost"; 
      29 $database_username = "cactiuser"; 
      30 $database_password = "kiwi888"; 
      31 $database_port = "3306";
      
    建立使用者cactiuser，密碼是設定為123456
    useradd cactiuser
    
    切到cactiuser
    su - cactiuser
    
    設定排程 cacti 每1分鐘收集一次數據    
    crontab -e
    輸入
    */1 * * * * /usr/local/bin/php  /home/cacti/poller.php > /dev/null 2>&1
    
    exit 離開cactiuser


***

{{< css.inline >}}
<style>
.emojify {
	font-family: Apple Color Emoji, Segoe UI Emoji, NotoColorEmoji, Segoe UI Symbol, Android Emoji, EmojiSymbols;
	font-size: 2rem;
	vertical-align: middle;
}
@media screen and (max-width:650px) {
  .nowrap {
    display: block;
    margin: 25px 0;
  }
}
</style>
{{< /css.inline >}}
