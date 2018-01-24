---
layout: layout.pug
navigationTitle: Converting Agent Node Types
title: Converting Agent Node Types
menuWeight: 700
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

You can convert agent nodes to public or private for an existing DC/OS cluster.

Agent nodes are designated as [public](/1.10/overview/concepts/#public-agent-node) or [private](/1.10/overview/concepts/#private-agent-node) during installation. 默认情况下, 它们在 [ gui ](/1.10/installing/oss/custom/gui/) 或 [ cli ](/1.10/installing/oss/custom/cli/) 安装期间被指定为专用。

### 基础要求

These steps must be performed on a machine that is configured as a DC/OS node. Any tasks that are running on the node will be terminated during this conversion process.

- DC/OS is installed using the [custom](/1.10/installing/oss/custom/) installation method and you have deployed at least one [master](/1.10/overview/concepts/#master) and one [private](/1.10/overview/concepts/#private-agent-node) agent node.
- The archived DC/OS installer file (`dcos-install.tar`) from your [installation](/1.10/installing/oss/custom/gui/#backup). 
- CLI JSON 处理器 [ jq ](https://github.com/stedolan/jq/wiki/Installation)。
- 已安装和配置 SSH。这是访问 DC/OS 群集中的节点所必需的。

### Determine the node type

You can determine the node type by running this command from the DC/OS CLI.

- Run this command to determine how many private agents are there in the cluster. A result of `` indicates that there are no private agents.
    
    ```bash
dcos node --json | jq --raw-output '.[] | select(.reserved_resources.slave_public == null) | .id' | wc -l
```

- Run this command to determine how many public agents are there in the cluster. A result of `` indicates that there are no public agents.
    
    ```bash
dcos node --json | jq --raw-output '.[] | select(.reserved_resources.slave_public != null) | .id' | wc -l
```

### Uninstall the DC/OS private agent software

1. Uninstall DC/OS on the agent node.
    
    ```bash
sudo /opt/mesosphere/bin/dcos-shell
sudo -i pkgpanda uninstall
sudo systemctl stop dcos-mesos-slave
sudo systemctl disable dcos-mesos-slave
```

2. Remove the old directory structures on the agent node.
    
    ```bash
sudo rm -rf /etc/mesosphere /opt/mesosphere /var/lib/mesos /var/lib/dcos
```

3. 重新启动本游戏
    
    ```bash
sudo reboot
```

### Install DC/OS and convert agent node

将已存档的 DC/OS 安装程序文件 (` dcos-install.tar `) 复制到代理节点。 This archive is created during the GUI or CLI [installation](/1.10/installing/oss/custom/gui/#backup) method.

1. 将文件复制到代理节点。例如, 可以使用安全副本 (scp) 将 `dcos-install.tar` 复制到您的主目录中:
    
    ```bash
scp ~/dcos-install.tar $username@$node-ip:~/dcos-install.tar
```

2. SSH 到机器:
    
    ```bash
ssh $USER@$AGENT
```

3. 为安装程序文件创建一个目录:
    
        bash
         sudo mkdir -p /opt/dcos_install_tmp

4. 解 `dcos-install.tar` 文件:
    
    ```bash
sudo tar xf dcos-install.tar -C /opt/dcos_install_tmp
```

5. 运行此命令可在代理节点上安装 DC/OS。您必须将代理节点指定为公共或专用。
    
    公共代理节点:
    
    ```bash
sudo bash /opt/dcos_install_tmp/dcos_install.sh slave
```

公共代理节点:

```bash
sudo bash /opt/dcos_install_tmp/dcos_install.sh slave_public
```