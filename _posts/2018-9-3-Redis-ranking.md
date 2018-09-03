---
layout:     post
title:      "用Redis实现排行榜（积分一致时按时间排）"
date:       2018-9-3 18:06:20
author:     "dxc"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
tags:
    - Redis
    - 服务器
---
# 用Redis实现排行榜（积分一致时按时间排） #

将（玩家，积分）存入Redis：
```java
zadd(KEY, playerId, score);
```
Redis的zset结构的每个成员都只关联一个元素score，要想依据多个元素（积分、时间）排序，必须将积分和时间都揉进score里，这就需要一个“编码”过程：
```java
public static final int RANKING_FACTOR = 100000000;

public static double encrypt(int score){
    double value = score * RANKING_FACTOR + (RANKING_FACTOR - System.currentTimeMillis()/1000 % RANKING_FACTOR);
    return value;
}
```
取出Redis中的排行数据：
```java
rankingList = zRevRange(KEY, start, end);
```
如果用到的信息较多，可以加一个hset结构来存储这些信息，从zset结构中获取playerId，在hset结构中查找。