---
layout: layout.pug
navigationTitle: 使用虚拟IP地址
title: 使用虚拟IP地址
menuWeight: 10
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC/OS can map traffic from a single Virtual IP (VIP) to multiple IP addresses and ports. DC/OS vip 是 ** 基于名称的 **, 这意味着客户端使用服务地址而不是 IP 地址进行连接。

DC/OS 自动生成不与 IP vip 冲突的基于名称的 vip, 因此您不必担心冲突。 此功能允许在安装服务时自动创建基于名称的 vip。

指定的 VIP 包含以下组件:

* 私有虚拟IP地址
* 端口(服务可用的端口)
* 服务名

您可以从DC / OS GUI为应用程序分配一个VIP。 您在部署新服务时输入的值将被转换为这些Marathon应用程序定义条目：

* ` portDefinitions `如果不使用Docker容器
* ` portMappings `如果使用Docker容器

VIP遵循这个命名约定：

    <service-name>.marathon.l4lb.thisdcos.directory:<port>
    

### 先决条件

* VIP地址池是您的应用程序唯一的。

## 创建一个VIP

1. 在DC / OS [ GUI ](/1.10/gui/)上，点击**Services**，然后点击**RUN A SERVICE**。
    
    1. From the **Networking** tab, select **NETWORK TYPE** > **Virtual Network: dcos**.
    2. Expand **ADD SERVICE ENDPOINT** and provide responses for:
        
        * **CONTAINER PORT**
        * **SERVICE ENDPOINT NAME**
        * **PORT MAPPING**
        * **LOAD BALANCED SERVICE ADDRESS**
        
        As you fill in these fields, the service addresses that Marathon sets up will appear at the bottom of the screen. You can assign multiple VIPs to your app by clicking **ADD SERVICE ENDPOINT**.
        
        ![VIP service definition](/1.10/img/vip-service-definition.png)
        
        In the example above, clients can access the service at `my-service.marathon.l4lb.thisdcos.directory:5555`.
    
    3. Click **REVIEW & RUN** and **RUN SERVICE**.

You can click on the **Networking** tab to view networking details for your service.

![VIP output](/1.10/img/vip-service-definition-output.png)

For more information on port configuration, see the [Marathon ports documentation](/1.10/deploying-services/service-ports/).

## Using VIPs with DC/OS Services

Some DC/OS services, for example [Kafka](/services/kafka/), automatically create VIPs when you install them. The naming convention is: `broker.<service.name>.l4lb.thisdcos.directory:9092`.

Follow these steps to view the VIP for Kafka.

### Via the GUI

1. Click **Networking** > **Networks** and select **dcos**.
2. Select your task to view details.
    
    ![](/1.10/img/vip-service-details.png)

### Via the CLI

**Prerequisite:** The Kafka service and CLI must be [installed](/services/kafka/).

1. Run this command:
    
    ```bash
dcos kafka endpoints broker
```

The output should resemble:

```json
{
  "address": [
    "10.0.2.199:9918"
  ],
  "zookeeper": "master.mesos:2181/dcos-service-kafka",
  "dns": [
    "broker-0.kafka.mesos:9918"
  ],
  "vip": "broker.kafka.l4lb.thisdcos.directory:9092"
}
```

You can use this VIP to address any one of the Kafka brokers in the cluster.

## FAQs

### Connections seem to close at random times

This behavior is often experienced with applications that have long lived connections, such as databases (e.g. PostgreSQL). To fix, try turning on keepalives. The keepalive can be an application specific mechanism like a heartbeat, or something in the protocol like a TCP keepalive. A keepalive is required because a load balancer cannot differentiate between idle or dead connections as no packets are sent in either case. The default timeout depends on the kernel configuration, but is usually 5 minutes.