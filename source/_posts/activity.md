---
title: activityManager
date: 2018-04-12 18:08:09
tags: work
---

### ActivityManager使用

在android设备命令行通常使用am来启动service，activity，发送广播等

```
$ am start/startservice -n ｛包名｝/｛包名｝.{活动名称}
```

如：启动一个名叫MainActivity的活动

```
am start -n com.example.test/com.example.test.MainActivity
```

如果想要Intent带参数的话

```
am start -n xxxxx --es city "shanghai" --ei year 2018 --ez flag true
```

启动内置设置

```
am start -a android.settings.SETTINGS
```