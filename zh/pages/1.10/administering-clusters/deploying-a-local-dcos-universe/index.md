---
layout: layout.pug
navigationTitle: 部署本地Universe
title: 部署本地Universe
menuWeight: 1000
excerpt: ""
enterprise: false
preview: true
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

您可以在数据中心上安装和运行 DC/OS 服务, 而无需使用本地 [ 宇宙 ](https://github.com/mesosphere/universe) 进行 internet 访问。 您可以部署包括所有认证的软件包 (最简单), 或包括选定的包 (高级) 的本地宇宙。

**基础要求**

* 已安装[DC/OS CLI](/1.10/cli/install/)。

* 已登录到 DC/OS CLI。在 DC/OS 企业中, 您必须以 ` dcos: 超级用户 ` 权限登录。

** 注意: **由于宇宙压缩是超过 2 gb 的大小, 它可能需要一些时间下载到您的本地驱动器, 并上传到每个主机。

# <a name="certified"></a>部署包含认证的宇宙包的本地宇宙

1. 在终端提示符下, 使用以下命令将本地宇宙及其服务定义下载到您的本地驱动器上。
    
    ```bash
curl -v https://downloads.mesosphere.com/universe/public/local-universe.tar.gz -o local-universe.tar.gz
curl -v https://raw.githubusercontent.com/mesosphere/universe/version-3.x/docker/local-universe/dcos-local-universe-http.service -o dcos-local-universe-http.service
curl -v https://raw.githubusercontent.com/mesosphere/universe/version-3.x/docker/local-universe/dcos-local-universe-registry.service -o dcos-local-universe-registry.service
```

2. 使用 [ 安全副本 ](https://linux.die.net/man/1/scp) 将宇宙和注册表文件传输到主节点, 在发出以下命令之前, 将 ` < 主-ip > ` 替换为主机的公共 ip 地址。
    
    **提示：**您可以在DC / OS Web界面的左上角找到主设备的公共IP地址。
    
    ```bash
scp local-universe.tar.gz core@<master-IP>:~
scp dcos-local-universe-http.service core@<master-IP>:~
scp dcos-local-universe-registry.service core@<master-IP>:~
```

3. 使用以下命令将[ SSH ](/1.10/administering-clusters/sshcluster/)添加到主服务器中。 用前面命令中使用的IP地址替换`< master-IP> `。
    
    ```bash
ssh -A core@<master-IP>
```

4. 确认文件已成功复制。
    
        ls
        

5. 您应该看到列出下列文件。
    
        dcos-local-universe-http.service  dcos-local-universe-registry.service  local-universe.tar.gz
        

6. 将注册表文件移动到` /etc/systemd/system/ `目录中。
    
        sudo mv dcos-local-universe-registry.service /etc/systemd/system/
        sudo mv dcos-local-universe-http.service /etc/systemd/system/
        

7. 确认文件已成功复制到`/etc/systemd/system/`中。
    
    ```bash
ls -la /etc/systemd/system/dcos-local-universe-*
```

8. 将Universe加载到本地Docker实例中。
    
    ```bash
docker load < local-universe.tar.gz
```

**提示**：这可能需要一些时间才能完成。

9. 重新启动systemd守护进程。
    
    ```bash
sudo systemctl daemon-reload
```

10. 启用并启动` dcos-local-universe-http `和` dcos-local-universe-registry `服务。
    
    ```bash
sudo systemctl enable dcos-local-universe-http
sudo systemctl enable dcos-local-universe-registry
sudo systemctl start dcos-local-universe-http
sudo systemctl start dcos-local-universe-registry
```

11. 使用以下命令确认服务已经启动并正在运行。
    
    ```bash
sudo systemctl status dcos-local-universe-http
sudo systemctl status dcos-local-universe-registry
```

12. 如果您只有一个主人，请跳到第25步。如果您有多个主人，请继续下一步。

13. 使用以下命令发现所有主设备的私有IP地址。 从列表中确定您现在被SSH连接的主机的私有IP地址。
    
    **提示：**在提示符中，它将匹配` core @ ip - `后面显示的路径，连字符将变成句点。
    
        host master.mesos
        

14. 使用[安全副本](https://linux.die.net/man/1/scp)将Universe和注册表文件转移到其他主人之一。 将`< master-IP>`替换为另一个主设备的IP地址。
    
    ```bash
scp local-universe.tar.gz core@<master-IP>:~
scp /etc/systemd/system/dcos-local-universe-registry.service core@<master-IP>:~
scp /etc/systemd/system/dcos-local-universe-http.service core@<master-IP>:~
```

15. 将[ SSH ](/1.10/administering-clusters/sshcluster/)添加到您刚刚将这些文件复制到的主文件夹中。
    
    ```bash
ssh -A core@<master_IP>
```

16. 确认文件已成功复制。
    
        ls
        

17. 您应该看到列出下列文件。
    
        dcos-local-universe-http.service  dcos-local-universe-registry.service  local-universe.tar.gz
        

18. 将注册表文件移动到` /etc/systemd/system/ `目录中。
    
        sudo mv dcos-local-universe-registry.service /etc/systemd/system/
        sudo mv dcos-local-universe-http.service /etc/systemd/system/
        

19. 确认文件已成功复制到`/etc/systemd/system/`中。
    
    ```bash
ls -la /etc/systemd/system/dcos-local-universe-*
```

20. 将Universe加载到本地Docker实例中。
    
        docker load < local-universe.tar.gz
        
    
    **提示**：这可能需要一些时间才能完成。

21. 重新启动Docker守护进程。
    
    ```bash
sudo systemctl daemon-reload
```

22. 启动` dcos-local-universe-http `和` dcos-local-universe-registry `服务。
    
    ```bash
sudo systemctl start dcos-local-universe-http
sudo systemctl start dcos-local-universe-registry
```

23. 使用以下命令确认服务已经启动并正在运行。
    
    ```bash
sudo systemctl status dcos-local-universe-http
sudo systemctl status dcos-local-universe-registry
```

24. Repeat steps 14 through 23 until you have completed this procedure for all of your masters. Then continue to the next step.

25. Close the SSH session by typing `exit` or open a new terminal prompt.
    
    **Tip:** You may have to exit more than one SSH session if you have multiple masters.

26. (Optional) Use the following command to remove the references to the default Universe from your cluster. If you want to leave the default Universe in place and just add the local Universe as an additional repository, skip to the next step.
    
    ```bash
dcos package repo remove Universe
```

**Tip:** You can also remove the references to the default Universe repository from **Settings** > **Package Repositories** in the DC/OS web interface.

27. Use the following command to add a reference to the local Universes that you added to each master.
    
    ```bash
dcos package repo add local-universe http://master.mesos:8082/repo
```

28. [SSH into one of your agent nodes.](/1.10/administering-clusters/sshcluster/)
    
    ```bash
dcos node ssh --master-proxy --mesos-id=<mesos-id>
```

29. Use the following commands to download a copy of the DC/OS certificate locally and set it as trusted.
    
    ```bash
sudo mkdir -p /etc/docker/certs.d/master.mesos:5000
sudo curl -o /etc/docker/certs.d/master.mesos:5000/ca.crt http://master.mesos:8082/certs/domain.crt
sudo systemctl restart docker
```

30. Configure the Apache Mesos fetcher to trust the downloaded Docker certificate.
    
    1. Copy the certificate:
        sudo cp /etc/docker/certs.d/master.mesos:5000/ca.crt /var/lib/dcos/pki/tls/certs/docker-registry-ca.crt
        
    
    1. Generate a hash:
        cd /var/lib/dcos/pki/tls/certs/
        openssl x509 -hash -noout -in docker-registry-ca.crt
        
    
    1. Create a soft link:
        sudo ln -s /var/lib/dcos/pki/tls/certs/docker-registry-ca.crt /var/lib/dcos/pki/tls/certs/<hash_number>.0
        
    
    **Note:** You will need to create the `/pki/tls/certs` directory on the public agent.

31. Close the SSH session by typing `exit` or open a new terminal prompt. Repeat steps 28-30 on each agent node.

32. To verify your success, log into the DC/OS web interface and click the **Catalog** tab. You should see a list of Certified packages. Install one of the packages.

### FAQ

* **I can't install CLI subcommands**
    
    Packages are hosted at `master.mesos:8082`. If you cannot resolve or connect to `master.mesos:8082` from your DC/OS CLI install, you cannot install CLI subcommands. If you can connect to port 8082 on your masters, add the IP for one of the masters to `/etc/hosts`.

* **The images are broken**
    
    All Universe components are hosted inside of your cluster, including the images. The components are served up by `master.mesos:8082`. If you have connectivity to that IP, you can add it to `/etc/hosts` and get the images working.

* **I don't see the package I was looking for**
    
    By default, only Certified packages are bundled. If you'd like to get something else, use the instructions in the next section.

# <a name="build"></a>Deploying a local Universe containing selected packages

**Prerequisite:** [Git](https://git-scm.com/). On Unix/Linux, see these <a href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git" target="_blank">installation instructions</a>.

To deploy a local Universe containing your own set of packages you must build a customized local Universe Docker image.

1. Clone the Universe repository:
    
    ```bash
git clone https://github.com/mesosphere/universe.git --branch version-3.x
```

2. Build the `universe-base` image:
    
    ```bash
cd universe/docker/local-universe/
sudo make base
```

3. Build the `mesosphere/universe` Docker image and compress it to the `local-universe.tar.gz` file. Specify a comma-separated list of package names and versions using the `DCOS_PACKAGE_INCLUDE` variable. To minimize the container size and download time, you can select only what you need. If you do not use the `DCOS_PACKAGE_INCLUDE` variable, all Certified Universe packages are included. To view which packages are Certified, click the **Catalog** tab in the DC/OS web interface.
    
    ```bash
sudo make DCOS_VERSION=1.10 DCOS_PACKAGE_INCLUDE="cassandra:1.0.25-3.0.10,marathon:1.4.2" local-universe
```

4. Perform all of the steps as described in [Deploying a local Universe containing Certified Universe packages](#certified), except step 27. Replace the command in step 27 with the following.
    
    ```bash
dcos package repo add local-universe http://master.mesos:8082/repo
```