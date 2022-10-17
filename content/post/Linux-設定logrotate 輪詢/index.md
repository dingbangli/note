+++
author = "Hugo Authors"
title = "Linux-設定logrotate 輪詢"
date = "2022-08-03"
#description = ""
categories = [
    "Linux"
]
tags = [
    "Logrotate",
]
image = "100.png"
+++


    mkdir /root/logrotate/
    
    ------------------------------------
    vim /root/logrotate/logrotate.sh
    
    #!/bin/bash
    
    /usr/sbin/logrotate -f /root/logrotate/weblog
    ------------------------------------
    vim /root/logrotate/weblog
    
    /usr/local/web/nginx/logs/*.acc {
        daily       #表示每天一份
        rotate 3    #設定保留份數
        create
        sharedscripts #加上這個指令的話就等於只會執行一次
        postrotate  #在 endscript 的區間內是 rotate 結束後會執行的指令
             /usr/local/web/nginx/sbin/nginx -s reload
        endscript
    }
    ------------------------------------
    crontab -e
    
    10 05 * * * sh /root/logrotate/logrotate.sh



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
