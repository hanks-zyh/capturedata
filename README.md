# capturedata

tools to capture network data  抓包工具总结




# 抓包工具 -- Charles

基于Java 跨平台： Linux , Mac OS X, Windows
[官网](https://www.charlesproxy.com/)

>Charles is an HTTP proxy / HTTP monitor / Reverse Proxy that enables a developer to view all of the HTTP and SSL / HTTPS traffic between their machine and the Internet. This includes requests, responses and the HTTP headers (which contain the cookies and caching information).

![](https://www.charlesproxy.com/assets/img/sm/23/image/mac_screen_321.png?k=f28bf8eff3)

----
**以下为Linux 平台**

## 安装

[下载 charles-proxy-3.11.2.tar.gz](https://www.charlesproxy.com/latest-release/download.do)

```
# 解压
tar zxvf charles-proxy-3.11.2.tar.gz

# 启动 charles
./charles/bin/charles
```

## 手机抓包

**前提：使手机和电脑在一个局域网内，不一定非要是一个ip段，只要是同一个路由下就可以了，比如电脑连接的有线网ip为192.168.16.12，然后手机链接的wifi ip为192.168.1.103，但是这个有线网和无线网的最终都是来自于一个外部ip，这样的话也是可以的。**

在安卓手机的 `设置 -> wlan -> 长按连接的wifi -> 修改网络 ->  高级选项 -> 代理选手动` ， 填入电脑的 ip 和 charles 监听的 端口（默认为 8888）

> 查看电脑端口 ： `ifconfig`
> 查看/修改 charles 监听端口： charles的 `Proxy -> Proxy Setting`

设置好之后Charles弹出确认框，点击Allow按钮即可

 ![图片](https://dn-coding-net-production-pp.qbox.me/ad193bbb-4d55-4feb-9e55-0634aa6dbfce.png) 

 ![图片](https://dn-coding-net-production-pp.qbox.me/c48ee820-f6ae-4814-b505-ff4870db1f5b.png) 



## 抓取 https

 ![图片](https://dn-coding-net-production-pp.qbox.me/027d34d1-c783-4aca-8db3-e0066bf4ca1b.png) 
 
### 安装证书
charles的 Help -> SSL Proxy -> Install charles Root

### 设置 ssl enable

在需要抓取的 https 链接上 右键 `enable ssl proxy`
![图片](https://dn-coding-net-production-pp.qbox.me/4b91cf2a-e35b-4b9b-96ac-047eafe607e6.png) 



参考 [charles使用教程指南](http://drops.wooyun.org/tips/2423)




# 抓包工具 -- mitmproxy

基于python 跨平台： linux, windows，	OSX (Mountain Lion and later)
[官网](https://mitmproxy.org/)

>Mitmproxy是一个基于python的中间人代理的框架。做过渗透测试的肯定很熟悉工具burpsuite或Fiddler，这些工具能够拦截并修改http或https的数据包，对于分析数据包交互的应用来说是非常有用的。但是这些工具都是整套给我们做好了。比如如果想自己定制一套这样的工具，添加一些自己需要的功能的话，那么我想，mitmproxy将是一个比较好的选择，因为它提供了一个可供用户调用的库libmproxy（注意该库目前只支持linux系统）。

![图片](https://dn-coding-net-production-pp.qbox.me/a2d09a30-ea72-42a1-818e-0982f7227784.png)

![图片](https://dn-coding-net-production-pp.qbox.me/85e1f4e1-c914-4ac1-9984-4492aad7096f.png)

![图片](https://dn-coding-net-production-pp.qbox.me/6992699a-cb21-44b7-9114-38d1b10f0272.png)

---
**以下基于Linux台（Ubuntu14.04）**

## 下载
```
$ sudo pip install mitmproxy

```
如果下载速度慢可以下载[tar包](https://mitmproxy.org/download/mitmproxy-0.14.0.tar.gz),需要netlib依赖， ** 推荐用pip 安装 **

## 启动

```
$ mitmproxy -b 192.168.1.29 -p 9999

```

输入 `？` 查看帮助， `q`返回
```
A      accept all intercepted flows
a      accept this intercepted flow
b      save request/response body
C      clear flow list or eventlog
d      delete flow
D      duplicate flow
e      toggle eventlog
F      toggle follow flow list
l      set limit filter pattern
L      load saved flows
m      toggle flow mark
n      create a new request
P      copy flow to clipboard
r      replay request
U      unmark all marked flows
V      revert changes to request
w      save flows
W      stream flows to file
X      kill and delete flow, even if it's mid-intercept
tab    tab between eventlog and flow list
enter  view flow
|      run script on this flow

```

手机设置好代理，界面上就可以看到请求了
![图片](https://dn-coding-net-production-pp.qbox.me/c5446754-1d45-4d7d-ba48-44b6e26d3b4f.png)

## mitmproxy 查看http请求响应

`C (大写)` 清除抓包结果

`j`  `k` 选择请求， 回车查看详细信息

 ![图片](https://dn-coding-net-production-pp.qbox.me/5cfa3fd1-879b-4db6-8f05-f8885d730964.png)

`tab`   切换 **Request** 和 **Response**

`m` Display Mode 美化信息
 ![图片](https://dn-coding-net-production-pp.qbox.me/70e7c4ad-f7c0-4e7a-bea6-2c4cdd36d50b.png)
 上图输入 m，在输入 s，便可以以json形式展示

`e` 编辑
Edit request (cookies,query,path,url,header,form,raw body,method)?
Edit response (cookies,code,message,header,raw body)?

### mitmproxy拦截 (Intercept)
相当于在客户端和服务器做中间人，可以修改客户端请求，修改服务器返回

输入 `i`（代表Intercept filter）即可，此时界面便会让你输入想要拦截的条件：

mitmproxy的条件拦截在默认情况下是过滤抓包的URL的。也就是说当你直接输入要拦截的条件（比如输入“weibo”），那么接下来要出现抓包会将匹配的抓包整体变黄：

mitmproxy条件过滤效果
mitmproxy条件拦截效果
这些橘黄色的数据包都代表被拦截了，还未发送给服务器，这个时候你就可以对这些数据包进行修改，我们选择一个数据包enter进入：

mitmproxy 拦截 选择数据包
与之前的类似，输入“e”，进行request编辑模式，然后输入“h”代表要编辑request的头部:

mitmproxy 编辑拦截包的头部
输入enter便可对高亮的User-Agent的值进行修改，上图的weibo版本之前是5.0的，被我改成了6.0 。我们还可以对header进行添加属性，输入“a”即可，然后使用tab分别键入key和value。这里我添加了“test－test”键值对：

mitmproxy 拦截header添加键值对
至此，我对拦截的request header已经修改完毕，现在要做的就是我要认可接受这个修改，然后发给服务器。所以我们输入“a”（代表“accept”）即可，等到服务器响应后，注意，mitmproxy便又了拦截服务器发过来的response（注意那个“Response intercepted”）：

mitmproxy 拦截response
现在如果你想修改这个response也可以，方式同上面修改request一样。这个时候我再输入“a”，代表我接受了这个response，然后这个response便可发给客户端了:

mitmproxy 拦截response之后accept

更多类型的mitmproxy拦截

同时mitmproxy还支持不同类型的条件过滤，之前在拦截字符串前面加上特定的参数比如我要拦截所有的POST request怎么办？输入：~m POST 即可（m代表method）：

mitmproxy 拦截特定的request 方法

拦截所有的request： ~q

拦截特定的header： ~h

拦截特定的domain： ~d

拦截特定的响应代码（404之类的）： ~c

mitmproxy官方文档。

-----


# 抓包工具 -- Fiddler

基于C#  windows, Linux看这里 [Mono Fiddler](http://fiddler.wikidot.com/mono)

---
## 下载
直接下载，安装即可

## 手机抓包
![](https://imququ.com/static/uploads/2013/09/Snip20130913_20.png.webp)


----

# 抓包工具 -- wireshark

跨平台：Windows，OS X ，Linux

[官网](https://www.wireshark.org/)

