+++
author = "Hugo Authors"
title = "AWS-EC2 mysql-slave跟隨RDS教學"
date = "2022-09-08"
#description = ""
categories = [
    "AWS"
]
tags = [
    "AWS",
]
image = "100.png"
+++



    將RDS Master的binlog保留時間延長至12小時
    CALL mysql.rds_set_configuration('binlog retention hours', 12);
    
    先記下RDS Master當前進度。
    show master status\G;
    
    建立一個臨時用的RDS-replica, 規格建議為 db.m3.xlarge 以上
    
    進入臨時用的RDS-replica內停止主從並記錄進度
    CALL mysql.rds_stop_replication;
    show slave status\G;
    
    匯出DB並排除幾個系統預設庫。
    mysql -e "show databases;" -uroot -p -hRDS-master連結位置 | grep -Ev "Database|XXX_mariadb|information_schema|innodb|mysql|performance_schema" | xargs mysqldump -uroot -p -hRDS-master連結位置 --databases > db.sql 
    
    RDS裡的mysql庫有幾個獨有的table，一般自己裝的mariaDB沒有那些table，需要獨立匯出特定那幾個table。
    mysqldump -uroot -p mysql rds_configuration rds_global_status_history rds_global_status_history_old rds_heartbeat2 rds_history rds_replication_status rds_sysinfo slow_log_template > RDS_mysql.sql
    
    停止EC2-Slave機的MySQL服務, 將mysql庫裡rds字頭的表通通刪掉, 外面全部的正式站的庫都刪掉(保留服務預設的庫)與白字檔(保留ib開頭檔案)後，再啟動服務。
    rm -f /var/lib/mysql/mysql/rds*
    rm -f [大寫英文字母開頭的目錄] [白字檔]
    mysqld_save&
    
    匯入資料庫
    mysql -uking -p < db.sql
    
    匯入獨有table
    mysql -uking -p mysql < RDS_mysql.sql
    
    檢查沒問題後，下跟隨
    CHANGE MASTER TO MASTER_HOST='RDS-master連結位置', MASTER_USER='主從帳號', MASTER_PASSWORD='主從密碼';  ----跟隨RDS-master
    CHANGE MASTER TO MASTER_LOG_FILE = 'bin-log檔名',MASTER_LOG_POS =進度;
    start slave;
    show slave status\G;




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
