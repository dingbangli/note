+++
author = "Hugo Authors"
title = "MariaDB-主從跟隨"
date = "2022-08-03"
#description = ""
categories = [
    "Database"
]
tags = [
    "MySQL",
]
image = "100.png"
+++



    Master:
    
    mysql -u root -p
    
    MariaDB [(none)]> STOP SLAVE;
    
    MariaDB [(none)]> GRANT REPLICATION SLAVE ON *.* TO '輸入使用者名稱'@'輸入來源 + %' IDENTIFIED BY '輸入密碼';
    
    FLUSH PRIVILEGES;
    
    FLUSH TABLES WITH READ LOCK;
    
    SHOW MASTER STATUS;
    
    exit;
    
    ############################################
    
    mysqldump --all-databases --user=root --password --master-data > masterdatabase.sql
    
    ############################################
    
    mysql -u root -p
    
    MariaDB [(none)]> UNLOCK TABLES;
    
    MariaDB [(none)]> exit
    
    ############################################
    
    scp masterdatabase.sql root@輸入slave主機來源:/home
    
    ############################################
    
    Slave:
    
    將master的SQL檔匯入slave
    mysql -u root -p < /home/masterdatabase.sql
    
    systemctl restart mariadb
    
    ############################################
    
    mysql -u root -p
    
    MariaDB [(none)]> STOP SLAVE;
    
    MariaDB [(none)]> CHANGE MASTER TO MASTER_HOST='輸入Master主機ip', MASTER_USER='輸入使用者名稱', MASTER_PASSWORD='輸入密碼', MASTER_LOG_FILE='mariadb-bin.000001', MASTER_LOG_POS=479;
    
    start slave;
    
    reset slave;
    
    SHOW SLAVE STATUS\G;
    
    ############################################



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
