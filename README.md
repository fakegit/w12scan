# w12scan
w12scan是一款网络资产搜索发现引擎，用于大规模搜索企业相关资产，也可当做小型Zoomeye使用。w12scan也是我的毕业设计，相关文档也会在答辩结束后上传。对于我来说，w12scan不再仅仅只是一个单纯的工具，有前面wscan系列扫描器的铺垫，w12scan可以更好的融合之前的项目，比如airbug，比如hack-requests，虽然单个看起来比较普通，但是融合在一起之后，会形成安全产品的闭环，可以类比看成编程工具当中的全家桶，在之后编写的项目中，也会有意识的将它们融入到这个体系中。

w12scan分为WEB端（用于展示显示数据）和Client端（用于搜索相关资产数据）。

这里是web端的开源程序，client端在[https://github.com/boy-hack/w12scan-client](https://github.com/boy-hack/w12scan-client)

一个视频了解W12SCAN


## 设计思想
w12scan网络资产发现引擎，该引擎基于python3 + django + elasticsearch 编写，目的是帮助你在浩瀚的网络空间中找到相应资产目标。

在程序设计中，整个系统由三大部分组成，WEB端，扫描端，和扫描中间件。WEB端用于展现数据与界面交互，扫描端只负责接收目标与返回结果，最终结果通过WEB restful API传递到WEB端。扫描中间件的任务是将一个目标通过各自信息收集手段拆分为多个ip端和多个目标域名，也负责整个扫描程序中ip，域名的去重作用，将最后结果提供到扫描端。

### 三者关系
```python
一个目标（eg：baidu.com） => 扫描中间件(各种方式搜索子域名和ip段相关资产，使用redis数据库去重) => 扫描端（扫描目标，返回结果）=> WEB端(接收扫描端提供数据，入库并分析)
```

## 特点

### WEB端
* 强大的搜索语法
    * 详见：xxx 通过限制cms名称，服务名称，标题，国家地区等等能够迅速找到相关目标。
* 自定义资产配置
    * 通过自定义某公司相关域名或ip资产，w12scan会自动帮你找到对应的资产目标，当你浏览该目标时，有醒目的标识提醒你该目标的归属。
* 强大的自动关联
    * 进入目标详情，若目标为ip，则会自动关联该ip上的所有域名和该c段上的所有域名。若目标为域名，则自动关联旁站，c段和子域名。

### 扫描端(Client)端
* 及时的poc验证
    * 通过对接airbug接口api，在线调用最新的poc验证脚本，airbug保证了漏洞更新的及时性，你也可以fork airbug项目后自行添加poc规则。
* 验证性攻击
    * 扫描端内置有常见的漏洞验证服务，每扫描一次网址，都会运行这些服务，结果最终会反馈到w12scan的WEB端展现。
* 扫描与识别
    * 端口扫描使用masscan，端口识别使用nmap，web应用识别调用wappalyzer和精简版的w11scan（指纹识别）
* 容易的分布式
    * 在程序架构设计就考虑到了这一点，扫描端只接受任务，最后的结果只和WEB端进行交互，所以在分布式上十分容易，直接在另一台机器上运行扫描端即可。也能很方便集成celery服务。

## 安装
- Docker安装
- Linux安装
- Windows安装

## 未来
* 有计划用`vue`或`react`重写前端，达到前后端分离
* 有计划能在WEB端增减插件
* 有计划能在WEB端增减指纹
* 有计划用go（并发支持）重写扫描模块（端口扫描，指纹识别等等）
* 有计划在扫描过程中集成特定的w9scan来扫描网站漏洞

## 法律
本程序主要用于收集网络数据用于分析研究，所发出的请求包不会对网络造成破坏。在使用该程序之前请遵守当地相关法律进行。