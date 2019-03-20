---
layout: post
title: nodejs 里边调用 mongodb4 事务时，提示Transaction numbers are only allowed on a replica set member or mongos
date: 2019-03-20 14:51:35 +UTC800
catetories: [mongodb nodejs]
tags: [mongodb nodejs]
---

第一次在nodejs里边调用mongodb4的transaction，但不顺利：
1. 刚开始调用aborttransaction不生效，经过观察所有的事务都不生效，也没有任何报错，结过搜索是因为在`insert({...})`中没有使用session，修改后为`inser({...}, {session: sessionVar})`
2. 继续运行修改后的代码，接着报 __Transaction numbers are only allowed on a replica set member or mongos__，开始看到这个错误无法理解，经过搜索查找，理解了这是需要一个replica set(副本集) 或者 mongos(切片)，replica set 搭建过程看[这儿](https://www.cnblogs.com/shengdimaya/p/6598450.html)
3. 在运行`rs.add('localhost:27018')`时报 __"errmsg" : "Quorum check failed because not enough voting nodes responded; required 2 but only the following 1 voting nodes responded: 127.0.0.1:27017;the following nodes did not respond affirmatively: 127.0.0.1:27018 failed with command replSetHeartbeat requires authentication"__，意思就是没有权限，复制集之间的互联也是需要验证的，所以要配置keyfile来满足这个需求，如果开启了 authorization ，投票节点通过证书的形式与复制集中其他节点进行认证。MongoDB的身份认证过程是加密的。MongoDB的认证交互是通过密码进行的