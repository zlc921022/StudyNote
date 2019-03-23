# DNS

    作用就是根据域名,查出对应的 IP 地址,它是 HTTP 协议的前提.

## 层次结构

    DNS 服务器的要求,一定是高可用,高并发和分布式的服务器.它被分为多个层次结构

    根 DNS 服务器
        返回顶级域 DNS 服务器的 IP 地址
    顶级域 DNS 服务器
        返回权威 DNS 服务器的 IP 地址
    权威 DNS 服务器
        返回相应主机的 IP 地址
## DNS 问题

    不稳定
        DNS 劫持或者故障,导致服务不可用
    不准确
        LocalDNS 调度,并不一定是就近原则,某些小运营商是没有自己的 DNS 服务器,直接调用其他运营商的 DNS 服务器,最终直接跨网传输
    不及时
        运营商可能会修改 DNS 的 TTL(缓存时间),导致DNS的修改,延迟生效
    

## HTTPDNS 的解决方案

    HTTPDNS 则不同,顾名思义它是利用 HTTP 协议与 NDS 服务器的 80 端口进行交互.不走传统的 DNS 解析,从而绕过运营商的 LocalDNS 服务器,有效的防止了域名劫持,提高域名解析的效率.


## OKHttp 接入 HTTPDNS

### OkHttp 默认的 DNS解析方式

    默认使用系统的 DNS 服务 InetAddress 进行域名解析

    要在OkHttp 中使用 HTTPDNS 有2种方式
        通过拦截器,在发送请求前,讲域名替换为 IP 地址
        通过 OkHttp 提供的 .dns() 接口,配置 HTTPDNS

### 拦截器接入

``` java
public class HTTPDNSInterceptor implements Interceptor {
    @Override
    public Response intercept(Chain chain) throws IOException {

        Request originRequest = chain.request();
        HttpUrl httpUrl = originRequest.url();

        String url = httpUrl.toString();
        String host = httpUrl.host();

        String hostIp = HTTPDNSHelper.getIpByHost(host);
        Request.Builder builder = originRequest.newBuilder();

        if (!TextUtils.isEmpty(hostIp)){
            builder.url(HTTPDNSHelper.getIpUrl(url,host,hostIp));
            builder.header("host",hostIp);
        }

        Request newRequest = builder.build();
        Response response = chain.proceed(newRequest);
        return response;
    }
}
```

#### 优缺点

    此方案（拦截器+HTTPDNS）遇到 https 时,如果存在一台服务器支持多个域名,可能导致证书无法匹配的问题

### .dns() 接入

``` java
public class HttpDns implements Dns {
    @Override
    public List<InetAddress> lookup(String hostname) throws UnknownHostException {
        String ip = HTTPDNSHelper.getIpByHost(hostname);
        if (TextUtils.isEmpty(ip)) {
            //返回自己解析的地址列表
            return Arrays.asList(InetAddress.getAllByName(ip));
        } else {
            // 解析失败，使用系统解析
            return Dns.SYSTEM.lookup(hostname);
        }
    }
}
```

#### 优缺点

    还是用域名进行访问,只是底层 DNS 解析换成了 HTTPDNS,以确保解析的 IP 地址符合预期.

    HTTPS 下的问题也得到解决,证书依然使用域名进行校验








[百度技术：“App 优化网络，先从优化 DNS 开始” | 原理到实战](https://mp.weixin.qq.com/s?__biz=MzIxNjc0ODExMA==&mid=2247486073&idx=1&sn=be2ae0427acdc714d6f236c3d65bb872&chksm=97851358a0f29a4eb6c9c8e73618813c97b9dc4608ef537ad1065ebad564253e744c8618e4a5&mpshare=1&scene=23&srcid=0314XPKcqRb4YpFzbbsCRfBo#rd)</br>

[百度App网络深度优化系列《一》DNS优化](https://mp.weixin.qq.com/s?__biz=MzUxMzk2ODI1NQ==&mid=2247483654&idx=1&sn=4cac066619fef001a1aed3616ad862af&scene=21#wechat_redirect)</br>