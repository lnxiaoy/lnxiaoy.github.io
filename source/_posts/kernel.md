---
title: kernel
date: 2018-04-24 21:50:42
tags: 负载均衡
---

### Kernel代码同步

| 名称 | Google GIT地址 | 清华服务器地址 |
| :-----: | :----: | :----: |
| common | https://android.googlesource.com/kernel/common.git | https://aosp.tuna.tsinghua.edu.cn/kernel/common.git |
| exynos | https://android.googlesource.com/kernel/exynos.git | https://aosp.tuna.tsinghua.edu.cn/kernel/exynos.git |
| goldfish | https://android.googlesource.com/kernel/goldfish.git | https://aosp.tuna.tsinghua.edu.cn/kernel/goldfish.git |
| hikey-linaro | https://android.googlesource.com/kernel/hikey-linaro | https://aosp.tuna.tsinghua.edu.cn/kernel/hikey-linaro.git |
| lk | https://aosp.tuna.tsinghua.edu.cn/kernel/lk.git |  |
| msm | https://android.googlesource.com/kernel/msm.git | https://aosp.tuna.tsinghua.edu.cn/kernel/msm.git |
| omap | https://android.googlesource.com/kernel/omap.git | https://aosp.tuna.tsinghua.edu.cn/kernel/omap.git |
| samsung | https://android.googlesource.com/kernel/samsung.git | https://aosp.tuna.tsinghua.edu.cn/kernel/samsung.git |
| tegra | https://android.googlesource.com/kernel/tegra.git | https://aosp.tuna.tsinghua.edu.cn/kernel/tegra.git |
| x86_64 | https://android.googlesource.com/kernel/x86_64.git | https://aosp.tuna.tsinghua.edu.cn/kernel/x86_64.git |
		
### 内核编译

编译前指定参数

```
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
```

为什么指定arm,指定arm64不行吗？
参考链接里有手把手的教程，鉴于太长了，就不一一配置了

[参考链接](https://blog.csdn.net/ffmxnjm/article/details/72933915)