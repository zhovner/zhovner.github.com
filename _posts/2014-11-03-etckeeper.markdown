---
layout: post
category: tools
tags: 
excerpt: 
title: 
---

### После установки etckeeper

В файле `/etc/etckeeper/etckeeper.conf` указать  `VCS="git"`  

`etckeeper init && etckeeper commit`  

`git config --global user.name "My Machine"`  
`git config --global user.email "etckeeper@my.machine"`  

### Создать приватный реп bitbucket.org
Если хочется хранить на удаленном сервере.

#### добавить ssh ключ
`cd /etc`  
`git remote add origin ssh://git@bitbucket.org/USERNAME/REPO-NAME.git`  

`echo “git show --stat” > /etc/.git/hooks/post-commit`  
`echo "git push origin master" >> /etc/.git/hooks/post-commit`  

`chmod +x /etc/.git/hooks/post-commit`

#### крон
`/etc/cron.daily/etckeeper`  

