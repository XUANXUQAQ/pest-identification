# pest-identification

第十届中国软件杯 A4 林业有害生物智能识别 国家二等奖

后端开发采用微服务架构，采用nacos作为服务的注册中心，下方为系统整体架构图

![architecture.png](https://raw.githubusercontent.com/XUANXUQAQ/pest-identification/main/architecture.png)

## 后端

nacos注册中心注册以下几个模块：

1. Pest-Identification-Provider
    提供主要的服务，包括数据库的基本操作和维护，神经网络反馈信息的处理和查询服    务等
2. Pest-Identification-Consumer
   提供查询接口，将与Provider对接，采用OpenFeign进行远程调用，进行负载均衡，保证系统的高可用性
3. Pest-Identification-Gateway
   提供网关接口，获取nacos的所有服务，并进行整合，对Consumer进行负载均衡，并    提高系统的灵活性，以后对系统的拓展也可以基于该网关配置

## 前端管理页面

后台管理前端采用vue编写，包含对数据库的基本增删改查，以及神经网络训练数据检查和上传，开始以及结束训练，修改训练参数，查看损失函数状态等功能。

## 图像识别

图像识别网络分为在线识别网络和离线识别网络

- 在线识别
  
  - 在线识别网络采用Yolov4实现，当上传图片后将会进行识别，并将识别后的结果返回。采用flask实现网络api，gunicorn进行多进程负载均衡，并将其接口整合进入spring cloud gateway。

- 离线识别
  
  - 离线识别网络采用Yolov5实现，通过在Android app本地打开一个微型http服务器，实现在线识别网络接口，在无网络时进行切换。然后在Android端使用Yolov5进行识别并返回。

## Android APP

Android app首先采用vue编写，之后使用CapacitorJS将vue转换为原生app，转换之后再通过nanoHTTPD本地模拟在线识别网络，实现Yolov5离线识别功能。
