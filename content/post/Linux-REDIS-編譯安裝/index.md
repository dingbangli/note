+++
author = "Hugo Authors"
title = "Linux-REDIS-編譯安裝"
date = "2022-08-03"
#description = ""
categories = [
    "Database"
]
tags = [
    "Redis",
]
image = "100.png"
+++



    ###################################
    
    http://download.redis.io/releases/
    
    ###################################
    
    wget - tar-zxvf - cd - 
    
    ###################################
    
    make MALLOC=libc
    
    make PREFIX=/usr/local/redis install
    
    ###################################
    
    Redis配置
    
    ###################################
    
    mkdir /usr/local/redis/etc/
    
    cp redis.conf /usr/local/redis/etc/
    
    cd /usr/local/redis/bin/
    
    cp redis-benchmark redis-cli redis-server /usr/bin/
    
    ###################################
    
    修改redis配置
    
    ###################################
    
    vim /usr/local/redis/etc/redis.conf
    
    bind 0.0.0.0
    
    daemonize yes
    
    ###################################
    
    vim redis ( 腳本 )
    
    ###################################
    
    #!/bin/bash
    #chkconfig: 2345 80 90
    # Simple Redis init.d script conceived to work on Linux systems
    # as it does use of the /proc filesystem.
    
    PATH=/usr/local/bin:/sbin:/usr/bin:/bin
    REDISPORT=6379
    EXEC=/usr/local/redis/bin/redis-server
    REDIS_CLI=/usr/local/redis/bin/redis-cli
    
    PIDFILE=/var/run/redis_6379.pid
    CONF="/usr/local/redis/etc/redis.conf"
    
    case "$1" in
    start)
    if [ -f $PIDFILE ]
    then
    echo "$PIDFILE exists, process is already running or crashed"
    else
    echo "Starting Redis server..."
    $EXEC $CONF
    fi
    if [ "$?"="0" ]
    then
    echo "Redis is running..."
    fi
    ;;
    stop)
    if [ ! -f $PIDFILE ]
    then
    echo "$PIDFILE does not exist, process is not running"
    else
    PID=$(cat $PIDFILE)
    echo "Stopping ..."
    $REDIS_CLI -p $REDISPORT SHUTDOWN
    while [ -x ${PIDFILE} ]
    do
    echo "Waiting for Redis to shutdown ..."
    sleep 1
    done
    echo "Redis stopped"
    fi
    ;;
    restart|force-reload)
    ${0} stop
    ${0} start
    ;;
    *)
    echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2
    exit 1
    esac
    
    ###################################
    
    cp redis /etc/init.d/
    
    chmod 755 /etc/init.d/redis
    
    ###################################
    
    chkconfig --list	                # 查看服務列表
    
    chkconfig --add redis		        # 添加服務
    
    chkconfig --level 2345 redis on		# 配制啟動級別
    
    ###################################
    
    systemctl start redis
    
    ###################################



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
