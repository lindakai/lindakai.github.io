---
layout: post
title: "火车票选铺记"
category: 生活
tags: Life
---

在暑假终于快结束的时候，大一的小朋友已经开始收拾行囊迎接全新的大学生活的时候，我的假期中终于要开始了……

早上买火车票的时候突然想起了以前好像在微博上看到过关于用Chrome浏览器选座位的帖子，于是突发奇想想要试试效果，于是到网上找了一份类似的资料（貌似也只找到一个版本）。但是审查元素后发现并没有找到帖子上说的代码，于是猜想可能是新版售票更新了代码？没有这么明显的漏洞了？

于是抱着一探究竟的态度继续看代码，发现网站的变量名称倒是很人性化，和座位相关的变量名基本都是"seat_???"，如 "seat_type","seat_type_name"等。

按照这个思路，就可以顺藤摸瓜找到铺位对应的"seat_detail_type_code",其默认值为NULL，在其附近也定义了各种和火车票信息相关的变量：

    /*<![CDATA[*/
       //common data
       var can_add = 'Y';
  	   var IsStudentDate=true;
       var init_seatTypes=[...];
       var defaultTicketTypes=[...];
       var init_cardTypes=[...];
       var ticket_seat_codeMap=[...];
       var ticketInfoForPassengerForm={...,‘seat_detail_type_code':null,...};
       var orderRequestDTO={...}；

这里就定义了与火车票相关的全部信息，大部分内容不需要关心，找到‘seat_detail_type_code'的位置后就可以通过修改后面的NULL值来选择上中下铺位了，其中：
        
|   seat_detail_type_code 的值 | 铺位  |
| :--:| :--: |
|    1 |  下铺  |
|    2 | 中铺  |    
|    3 | 上铺  |    

将NULL修改为自己想要铺位对应的代码，然后提交订单就能够买到想要的铺位啦~（当然前提是该铺位还有空余的票）。

        
        
        
        
        
        
