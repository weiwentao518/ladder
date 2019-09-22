# 史上最详细的梯子教程：CentOS 7.4 + 阿里云ECS

# CentOS 7.4 手把手教你快速搭梯子

**还学不会我也不吃屎**

> [参考博客 CSDN: 作者 Tony Fang](https://blog.csdn.net/qq_24279351/article/details/82421568)

# 买服务器

## 阿里云ECS云服务器

### 1. 基础配置
地域选择新加坡⬇️，实例选择最低配置即可。
PS：选择新加坡是因为比较稳定，如果选择国内的，你的IP随时由可能被封杀。
![基础配置1](https://upload-images.jianshu.io/upload_images/16025899-38b8d97d29d4970d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
镜像选择 公共镜像-CentOS-7.4 64位
云盘由40G改成20G
![基础配置2](https://upload-images.jianshu.io/upload_images/16025899-074b5f270f7db6f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2. 网络和安全组
选择“按使用流量” 拉满100M
![](https://upload-images.jianshu.io/upload_images/16025899-3dbdb7f6600b4fb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3. 系统配置
![](https://upload-images.jianshu.io/upload_images/16025899-0e069622f29fbda7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4. 分组设置（过）

### 5.确认订单
第一次可以先选择一周试试水，后面可以续费
![](https://upload-images.jianshu.io/upload_images/16025899-ef372acea1009c36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

买最低配置大概：27元/月
*   注意系统一定要选择centos 7.4版本，选错了遇到问题可不负责喔
*   硬盘 20G 1 vCPU 512 MB

## 弹性公网IP

避免封掉，可以释放掉重新申请再绑定。按量计费 100Mbps， 0.75元 /GB

# 服务器端配置

* * *

## 1. 远程登录

``ssh root@xx.xx.xx.xx``

![看见Welcome说明登陆成功了](https://upload-images.jianshu.io/upload_images/16025899-e9b8f49d40eb21f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 2. 查看系统版本

``cat /etc/redhat-release``
![](https://upload-images.jianshu.io/upload_images/16025899-7039e08e584fb494.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 3. 安装pip:

首先安装epel扩展源：

``yum -y install epel-release``

更新完成之后，就可安装pip：

``yum -y install python-pip``

安装完成之后清除cache：

``yum clean all``

升级pip到最新版本:

``pip install --upgrade pip``

## 4. 安装配置 shadowsocks

pip 安装 python 版本的 shadowsocks

``pip install shadowsocks``

安装完成后，需要创建shadowsocks的配置文件/etc/shadowsocks.json，编辑内容如下：

``vim /etc/shadowsocks.json``
```
{
  "server": "0.0.0.0", //这里不用改，全0代表地服务器监所有可用网络。
  "server_port": 8888, //服务器端口号，1025到65535任选一。
  "password": "mima123321", //设置登录密码。
  "method": "rc4-md5"//加密方式。
}
```
复制⬇️
```
{
  "server": "0.0.0.0",
  "server_port": 8888,
  "password": "mima123321",
  "method": "rc4-md5"
}
```

## 5. 配置自启动

编辑启动脚本：

``vim /etc/systemd/system/shadowsocks.service``
```
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

[Install]
WantedBy=multi-user.target
```

重新启动 shadowsocks 服务：

``systemctl enable shadowsocks``
``systemctl start shadowsocks``

查看状态：

``systemctl status shadowsocks -l``

![显示active(running)](https://upload-images.jianshu.io/upload_images/16025899-d8a0674dca3973ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这如果显示active(running)就表示服务器配置成功。没有成功的话可以返回“安装配置shadowsocks”的第2步开始认真按介绍再来一次。

配置安全组

我这里使用的是阿里云服务器，所以在服务器管理界面添加安全组规则，如下配置，然后确认，至此服务器端已大功告成。

![点击管理](https://upload-images.jianshu.io/upload_images/16025899-b2cc6133724dfa56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![点击添加安全组规则](https://upload-images.jianshu.io/upload_images/16025899-f64792e724cc04c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![配置安全组](https://upload-images.jianshu.io/upload_images/16025899-a369e63afad94286?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "image.png")

# 配置弹性公网IP

![](https://upload-images.jianshu.io/upload_images/16025899-d4313852a995b3ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后点击![](https://upload-images.jianshu.io/upload_images/16025899-5ffe0cec9b02ba23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![拉满到200M！让你速度🛫️](https://upload-images.jianshu.io/upload_images/16025899-04ca83ede258df07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

申请成功之后，先去ECS实例那里将公网IP转换为弹性公网IP

![](https://upload-images.jianshu.io/upload_images/16025899-f35f0760105e2878.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后在绑定弹性公网IP

![](https://upload-images.jianshu.io/upload_images/16025899-c6349c116ddde017.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



# <客户端配置>

## 下载地址：
Windows:
https://github.com/shadowsocks/shadowsocks-windows/releases

MAC:
https://github.com/shadowsocks/ShadowsocksX-NG/releases

Android：
https://github.com/shadowsocks/shadowsocks-android/releases

## Mac
密码就是你编辑``/etc/shadowsocks.json``文件的时候，设置的登陆密码

![](https://upload-images.jianshu.io/upload_images/16025899-fff06921145b6ca2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## Windows
![](https://upload-images.jianshu.io/upload_images/16025899-82ef2753e4d9c759.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "image.png")

*   服务器地址：填写服务器公网地址
*   服务器端口：填写服务器SS配置的端口
*   密码：填写服务器配置的密码
*   加密：填写和服务器配置一致的密码
*   其他不用管，然后继续

然后右键任务栏的小飞机，点击启用系统代理

PAC 自动模式：主要根据文件里面的规则来访问网络，及被屏蔽的网站才走代理访问。PAC 里面规则根据 [GFWList](https://github.com/gfwlist/gfwlist) 生成。

如果首次使用 **PAC 自动模式** 无法访问，请选择 **全局模式**，在选择 **从 GFW List 更新 PAC**，更新成功后重新选择 **PAC 自动模式**。

## iPhone

切换到美区使用：Potatso Lite 或者Shadowrocket，选择智能路由

# 快去Google一下试试效果吧～
[https://www.google.com.hk/](https://www.google.com.hk/)

![](https://upload-images.jianshu.io/upload_images/16025899-accaf837d3f4963e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

