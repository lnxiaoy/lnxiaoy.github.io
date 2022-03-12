---
title: android刷新dns缓存
date: 2018-03-20 18:08:09
tags: work
---

### 查看dns信息

```
nslookup xxxx.com
```

### 刷新缓存

```
//开启飞行模式
settings put global airplane_mode_on 1
am broadcast -a android.intent.action.AIRPLANE_MODE --ez state true
//关闭飞行模式
settings put global airplane_mode_on 0
am broadcast -a android.intent.action.AIRPLANE_MODE --ez state false
```