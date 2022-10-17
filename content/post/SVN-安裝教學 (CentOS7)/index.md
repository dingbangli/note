+++
author = "Hugo Authors"
title = "SVN-安裝教學 (CentOS7)"
date = "2022-10-06"
#description = ""
categories = [
    "SVN"
]
tags = [
    "SVN",
]
image = "100.png"
+++


    wget https://archive.apache.org/dist/subversion/subversion-1.6.12.tar.gz
    
    yum install apr*
    yum install sqlite
    
    ./configure --prefix=/usr/local/svn --without-berkeley-db
    make && make install
    
    啟動 svnserve:
    svnserve -d
    
    adduser svn
    passwd svn
    
    建立資料庫
    svnadmin create /home/svn/project/
    
    增加使用者與密碼
    vim /home/svn/project/conf/passwd
    
    [users]
    XXX=XXX
    
    
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
