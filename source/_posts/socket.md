---
title: socket
date: 2018-04-11 18:08:09
tags: work
---

### 网络编程

在进行网络编程，文件下载的时候，遇见了以下问题，记录一下，框架使用okHttp3

#### java.net.SocketException: Socket is closed

该异常在客户端和服务器均可能发生。异常的原因是己方主动关闭了连接后（调用了Socket的close方法）再对网络连接进行读写操作。

在有多个服务的系统中，发现如果有别的服务打开过socket链接，在链接服务器不成功的情况下，退出并关闭了socket链接，有可能是我正在使用的端口，×端口有没有可能共享×，或者只要保证【Server IP + Server Port + Client IP + Client Port】这个组合唯一不重复即可。

#### Exception: Unexpected end of Stream

客户端中读取到的字节为0,考虑服务器中断了，CDN节点切换了等导致网络不畅

#### java.net.ConnectException: Connection refused

访问服务器给定的地址，链接被拒绝了，考虑服务器暂停了这个服务

#### 总结

使用okHttp框架，添加如下代码也不能进行重试，只能放在try catch里捕捉进行手动重试了。

``` java
public static OkHttpClient getmClient() {
        if (mOkHttpClinet == null) {
            synchronized (OkHttpClientHelper.class) {
                if (mOkHttpClinet == null) {
                    OkHttpClient.Builder mBuilder = new OkHttpClient.Builder();
                    mBuilder.sslSocketFactory(createSSLSocketFactory(), new TrustAllCerts());
                    mBuilder.retryOnConnectionFailure(true);
                    mBuilder.addNetworkInterceptor(new Interceptor() {
                        @Override
                        public Response intercept(Chain chain) throws IOException {
                            Request request = chain.request().newBuilder().addHeader("Connection", "close").build();
                            return chain.proceed(request);
                        }
                    });
                    mBuilder.hostnameVerifier(new TrustAllHostnameVerifier());
                    mOkHttpClinet = mBuilder.build();
                }
            }
        }
        LogUtils.d(TAG, "Single OkHttpClient");
        return mOkHttpClinet;
    }
```
