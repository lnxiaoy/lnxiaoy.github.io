---
title: 猿宵节
date: 2018-03-03 18:08:09
tags: work
---

### 2018年在上海的第一个元宵节

古人有云：
猿宵节是程序员在元宵佳节通宵加班的日子
结果这一天还真是这个样子了，主要解决×so热更新的问题×

#### so热更新

目前应用结构为apk->xxx.jar->xxxJNI.so->xxx.so，native层已经具备so的检查和下载，缺少的是so的替换过程，处理逻辑上

1. 将上一个运行着的进程deinit，目前会崩溃
2. 将新的so拷贝为一个位置（这个位置有特定要求）
3. 重新init新的进程

#### so拷贝路径

1. 将so拷贝到/data/data/包名下的cache文件夹内，重新调用so文件时，提醒system_app没有excute权限
解决办法：将so拷贝到外置存储目录Environment.Downloads下。
2. 重新调用so时，提示dlopen failed:不能调用so，调用路径只能为以下5个中的一个{/data, /system/lib/, /system/vendor/lib/, …}，有的没记住
解决办法：将so拷贝到/data/app_lib下，赋予权限
3. 重新调用so时，还是提示没有excute权限
解决办法：手动创建/data下的一个文件，赋予权限
4. 依然无效后，通过log，找到app.te，在其中neverallow里将system_app从黑名单中删除，解决问题
