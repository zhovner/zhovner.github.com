---
layout: post
category: geek
tags: 
excerpt: 
title: "How To Create P2P-based Conference in New Skype"
---

In modern Skype versions was added new conference type called "Cloud-based chat room".  
This new conferences haven't creator role, and many old options not work.  

Old style conferences now called P2P-based chat.  
Each type has different set of commands:  [http://www.skype.com/go/help.chathelp](http://www.skype.com/go/help.chathelp)  
Cloud-based chat not supports options `TOPIC_AND_PIC_LOCKED_FOR_USERS` and `ADDING_LOCKED_FOR_USERS`.
This means that you can't protect topic and can't disable adding new users for everyone.  


To identify conference type enter the command `/get creator`  

in Cloud-based chat it returns:  

`System: creator=Unknown`

in P2P-based it returns your username.

The only way I found how to create P2P conference is to login through old Skype 5.5.  
But Skype disable old clients, so you need to download patched 5.5 version that appear as 6.x.x version.

Magnet link of pathced 5.5 version:

`magnet:?xt=urn:btih:8003250D6A73DC766B5937C3DA28755FEE67D9E7`

P2P conference that created in old Skype will work in new versions as well.

Skype for Linux (4.3.0.37) can also create only P2P chats.