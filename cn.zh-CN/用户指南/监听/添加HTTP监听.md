# 添加HTTP监听

HTTP协议适用于需要对数据内容进行识别的应用，如Web应用和小的手机游戏等。您可以添加一个HTTP监听转发来自HTTP协议的请求。

您已经创建负载均衡实例，详情请参见[创建负载均衡实例](/cn.zh-CN/用户指南/实例/创建负载均衡实例.md)。

## 步骤一：打开监听配置向导

完成以下操作，打开监听配置向导。

1.  登录[负载均衡管理控制台](https://slb.console.aliyun.com/slb)。

2.  在左侧导航栏，选择**实例** \> **实例管理**。

3.  选择实例的地域。

4.  选择以下一种方法，打开监听配置向导。

    -   在实例管理页面，找到目标实例，单击**监听配置向导**。

        ![实例管理](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5772129951/p161626.png)

    -   在实例管理页面，单击目标实例ID。在监听页面，单击**添加监听**。

        ![添加监听](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5772129951/p7399.png)


## 步骤二：配置协议监听

完成以下操作， 配置协议监听：

1.  在协议&监听页面，根据以下信息配置HTTP监听。

    |监听配置|说明|
    |:---|:-|
    |**选择负载均衡协议**|选择监听的协议类型。 本示例选择**HTTP**。 |
    |**监听端口**|用来接收请求并向后端服务器进行请求转发的监听端口。 端口范围为1~65535。

**说明：** 在同一个负载均衡实例内，监听端口不可重复。 |
    |**高级配置**|
    |**调度算法**|负载均衡支持轮询 \(RR\)、加权轮询 \(WRR\)、加权最小连接数 \(WLC\)三种调度算法。     -   **加权轮询 \(WRR\)加权轮询**：权重值越高的后端服务器，被轮询到的次数（概率）也越高。
    -   **轮询 \(RR\)**：按照访问顺序依次将外部请求分发到后端服务器。
    -   **加权最小连接数 \(WLC\)**：除了根据每台后端服务器设定的权重值来进行轮询，同时还考虑后端服务器的实际负载（即连接数）。将访问请求分发给当前连接数最小的后端服务器，权重越大的被分发的几率越大。 |
    |**监听转发**|选择是否将HTTP监听的流量转发到HTTPS监听。 **说明：** 如果开启监听转发，确保您已经创建了HTTPS监听。 |
    |**开启会话保持**|选择是否开启会话保持。

开启会话保持功能后，负载均衡会把来自同一客户端的访问请求分发到同一台后端服务器上进行处理。

HTTP协议会话保持基于Cookie。负载均衡提供了两种Cookie处理方式：

    -   **植入Cookie**：您只需要指定Cookie的过期时间。

客户端第一次访问时，负载均衡会在返回请求中植入Cookie（即在HTTP/HTTPS响应报文中插入SERVERID），下次客户端携带此Cookie访问，负载均衡服务会将请求定向转发给之前记录到的后端服务器上。

    -   **重写Cookie**：可以根据需要指定HTTPS/HTTP响应中插入的Cookie。您需要在后端服务器上维护该Cookie的过期时间和生存时间。

负载均衡服务发现用户自定义了Cookie，将会对原来的Cookie进行重写，下次客户端携带新的Cookie访问，负载均衡服务会将请求定向转发给之前记录到的后端服务器。详情请参见[会话保持规则配置]()。 |
    |**启用访问控制**|选择是否启用访问控制。|
    |**访问控制方式**|开启访问控制后，选择一种访问控制方式：

    -   **白名单：允许特定IP访问负载均衡SLB**，仅转发来自所选访问控制策略组中设置的IP地址或地址段的请求，白名单适用于应用只允许特定IP访问的场景。

设置白名单存在一定业务风险。一旦设置白名单，就只有白名单中的IP可以访问负载均衡监听。如果开启了白名单访问，但访问策略组中没有添加任何IP，则负载均衡监听会转发全部请求。

    -   **黑名单：禁止特定IP访问负载均衡SLB**，来自所选访问控制策略组中设置的IP地址或地址段的所有请求都不会转发，黑名单适用于应用只限制某些特定IP访问的场景。

如果开启了黑名单访问，但访问策略组中没有添加任何IP，则负载均衡监听会转发全部请求。 |
    |**选择访问控制策略组**|选择访问控制策略组，作为该监听的白名单或黑名单。 **说明：** IPv6实例只能绑定IPv6访问控制策略组，IPv4实例只能绑定IPv4访问控制策略组。详情请参见[创建访问控制策略组](/cn.zh-CN/用户指南/访问控制/访问控制策略组/创建访问控制策略组.md)。 |
    |**开启监听带宽限速**|选择是否配置监听带宽。

对于按带宽计费的负载均衡实例，您可以针对不同监听设定不同的带宽峰值来限定监听的流量。实例下所有监听的带宽峰值总和不能超过该实例的带宽。

默认不开启，各监听共享实例的总带宽。

**说明：** 使用流量计费方式的实例默认不限制带宽峰值。 |
    |**连接空闲超时时间**|指定连接空闲超时时间，取值范围为1~60秒。 在超时时间内一直没有访问请求，负载均衡会暂时中断当前连接，直到下一次请求来临时重新建立新的连接。

该功能已经在全部地域开放。

**说明：** 该功能对使用HTTP/2.0的请求暂不生效。 |
    |**连接请求超时时间**|指定请求超时时间，取值范围为1~180秒。 在超时时间内后端服务器一直没有响应，负载均衡将放弃等待，给客户端返回HTTP 504错误码。

该功能已经在全部地域开放。 |
    |**Gzip数据压缩**|开启该配置对特定文件类型进行压缩。 目前Gzip支持压缩的类型包括：text/xml、text/plain、text/css、application/javascript、application/x-javascript、application/rss+xml、application/atom+xml和application/xml。 |
    |**附加HTTP头字段**|选择您要添加的自定义HTTP header字段：     -   添加`X-Forwarded-For`字段获取客户端的IP地址。
    -   添加`X-Forwarded-Proto`字段获取实例的监听协议。
    -   添加`SLB-IP`字段获取负载均衡实例的公网IP。
    -   添加`SLB-ID`字段获取负载均衡实例的ID。 |
    |**获取客户端真实IP**|HTTP监听通过 X-Forwarded-For获取客户端真实IP。|
    |**创建完毕自动启动监听**|是否在监听配置完成后启动负载均衡监听，默认开启。|

2.  单击**下一步**。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2772129951/p7434.png)


## 步骤三：添加后端服务器

添加处理前端请求的后端服务器。您可以使用实例配置的默认服务器组，也可以为监听配置一个虚拟服务器组或主备服务器组。详情请参见[后端服务器概述](/cn.zh-CN/用户指南/后端服务器/后端服务器概述.md)。

本操作中，以默认后端服务器组为例：

1.  选择**默认服务器组**，单击**继续添加**。

    ![添加默认服务器](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5772129951/p10030.png)

2.  在**我的服务器**页面，选择要添加的ECS实例，然后单击**下一步**。

    ![配置权重](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5772129951/p7499.png)

3.  在**配置端口和权重**页签下，配置添加的后端服务器的权重，权重越高的ECS实例将被分配到更多的访问请求。

    **说明：** 权重设置为0，该服务器不会再接受新请求。

4.  单击**添加**，配置后端服务器（ECS实例）开放用来接收请求的端口，端口范围为1~65535。

    同一个负载均衡实例内，后端服务器端口可以相同。

5.  单击**下一步**。


## 步骤四：配置健康检查

负载均衡通过健康检查来判断后端服务器（ECS实例）的业务可用性。健康检查机制提高了前端业务整体可用性，避免了后端ECS异常对总体服务的影响。单击**修改**更改健康检查配置，详情请参见[健康检查概述](/cn.zh-CN/用户指南/健康检查/健康检查概述.md)。

## 步骤五：提交配置

完成以下操作，确认监听配置。

1.  在配置审核页面，检查监听配置，您可以单击**修改**更改配置。

2.  确认无误后，单击**提交**。

3.  在配置成功页面，配置成功后，单击**知道了**。

    配置成功后，您可以在监听页面查看已创建的监听。


**相关文档**  


[添加ECS实例作为默认服务器](/cn.zh-CN/用户指南/后端服务器/默认服务器组/添加默认服务器.md)

[配置健康检查](/cn.zh-CN/用户指南/健康检查/配置健康检查.md)

[添加ECS实例作为虚拟服务器](/cn.zh-CN/用户指南/后端服务器/虚拟服务器组/添加ECS实例作为虚拟服务器.md)

[添加ECS实例作为主备服务器](/cn.zh-CN/用户指南/后端服务器/主备服务器/创建主备服务器组.md)

[访问控制概述](/cn.zh-CN/用户指南/访问控制/访问控制概述.md)

[基于域名或URL路径进行转发](/cn.zh-CN/教程专区/基于域名或URL路径进行转发.md)

[概述](/cn.zh-CN/用户指南/监听/扩展域名/概述.md)

[CreateLoadBalancerHTTPListener](/cn.zh-CN/开发指南/API参考/HTTP监听/CreateLoadBalancerHTTPListener.md)

