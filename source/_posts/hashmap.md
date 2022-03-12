---
title: hashMap
date: 2017-12-25 18:08:09
tags: work
---

调用hashmap的put方法时，会通过一个哈希函数来确定插入位置，有可能引起冲突，冲突时，在对应位置使用”头插法”插入链表,后被插入的Entry被找到的可能性更大
调用hashmap的get方法时，通过哈希函数做映射，查找对应位置，如果有多个，则顺着链表一直向下查找

默认初始长度为16,每次自动扩展或者手动初始化都是2的幂，之所以是16,是因为key到index的hash算法要尽量均匀分布，通过利用key的hascode值和hashmap的长度做位运算。

index = HashCode(Key) & (Length - 1)

hash最终的index结果几乎取决于Key的hascode后几位

TODO：
[ ] 高并发下死锁
[ ] java8中的优化