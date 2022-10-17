+++
author = "Hugo Authors"
title = "AWS-VPC 建置"
date = "2022-08-05"
#description = ""
categories = [
    "AWS"
]
tags = [
    "AWS",
]
image = "100.png"
+++


    1.建立1個VPC
    
    2.新增4個subnets (2個Public、2個private) 用途:預防AZ出現故障無法切換

    3.新增Internet gateways，並將IGW掛上VPC。

    4.新增NAT gateways，選擇Public subnet，並給一個外網IP。

    5.新增2個route table，1個叫igw、1個叫nat。

    6.subnets 套用route talbe，Public套用igw，private套用nat。


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
