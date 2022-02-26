# curl基础

---

**参考：**

```crystal
# 阮一峰
https://www.ruanyifeng.com/blog/2011/09/curl.html

# catonmat.net
https://catonmat.net/cookbooks/curl

# 官方网站
https://curl.se/
https://everything.curl.dev/
```

## curl是啥

- curl 是常用的命令行工具，用来请求 Web 服务器。
- 它的名字就是客户端（client）的 URL 工具的意思。
- 它的功能非常强大，命令行参数多达几十种。如果熟练的话，完全可以取代 Postman 这一类的图形界面工具。

## curl命令

get请求中有多个参数时，必须用 `" "` 包裹。否则只能识别第一个参数：

```shell
curl "http://ws.webxml.com.cn/WebServices/WeatherWS.asmx/getWeather?theCityCode=%E6%B7%B1%E5%9C%B3&theUserID="
```

