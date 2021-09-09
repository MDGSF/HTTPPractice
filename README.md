# HTTPPractice

## 自签名证书

```sh
openssl genrsa -out privkey.pem 2048
得到新的私钥privkey.pem

openssl req -new -x509 -sha256 -key privkey.pem -out cert.pem -days 365 -subj "/CN=fake.example.org"
得到新的证书cert.pem
```

启动一个本地http/2服务

```
sudo apt-get install nghttp2

nghttpd -v -d <webroot> <port> <key> <cert>
<webroot> 是网站目录
<port> 端口
<key> 私钥
<cert> 证书

nghttpd -v -d . 8443 privkey.pem cert.pem

浏览器访问 https://0.0.0.0:8443/index.html
```

## HTTP/2 提供两种协议发现的机制。

在连接不加密的情况下，客户端会利用 Upgrade 首部来表明期望使用 h2。如果服务器
也可以支持 h2，它会返回一个“101 Switching Protocols”（协议转换）响应。这增加了
一轮完整的请求－响应通信。

如果连接基于 TLS， 情 况 就 不 同 了。 客 户 端 在 ClientHello 消息中设置 ALPN
（Application-Layer Protocol Negotiation，应用层协议协商）扩展来表明期望使用 h2 协
议，服务器用同样的方式回复。如果使用这种方式，那么 h2 在创建 TLS 握手的过程中
完成协商，不需要多余的网络通信。值得注意的是，SPDY 和 h2 的早期修订版本使用
NPN（Next Protocol Negotiation，下一代协议协商）扩展来完成 h2 协商。它在 2014 年
中期被 ALPN 取代。

## 发送http/2请求

```sh
nghttp -v -n --no-dep -w 14 -a -H "Header1: Foo" https://www.akamai.com
这条命令把窗口大小设置为 16KB（214），添加了一个没有意义的首部，并请求下载页面的
一些关键资源。
```

## tcpdump

```sh
sudo tcpdump -i lo -w log.cap
```

## curl编译支持http3

https://github.com/curl/curl/blob/master/docs/HTTP3.md

## http3测试网站

https://bagder.github.io/HTTP3-test/
https://github.com/bagder/HTTP3-test






