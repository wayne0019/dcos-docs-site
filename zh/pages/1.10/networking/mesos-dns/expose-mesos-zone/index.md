---
layout: layout.pug
navigationTitle: 外部暴露 Mesos 区域
title: 外部暴露 Mesos 区域
menuWeight: 300
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

有些情况下, 您可能希望在 dc/os 之外使用 dc/os 群集内部的 DNS 记录的服务。 但是, DC/OS 用于公开记录的 `. mesos ` 域名称不支持此项。 要启用此功能, 您可以将 BIND 服务器放在群集的前面。

每个 DC/OS 群集都有一个唯一的加密标识符。可以在 ** 概述 ** 下的 UI 中找到该标识符的 zbase32 编码版本。

In the example, the cryptographic cluster ID `yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo` is used.

1. Install a BIND server in front of your cluster.

2. Create a forwarding entry for your DC/OS master that resembles this.
    
        zone "yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo.dcos.directory" {
                type forward;
                forward only;
                forwarders { 10.0.4.173; };  // <Master-IP-1;Master-IP-2;Master-IP-3>
        };
        

3. Replace the master IP (`<Master-IP>`) with a semicolon separated list of your own master IPs.

4. Replace the example cryptographic cluster ID with your own.

## Making a zone

Now, you can create the zone that you'd like to alias to this. You can also skip this step and [use an existing zone](#existing).

1. Create a zone entry in the `named.conf` file. For this example, `contoso.com` is used:
    
        zone "contoso.com" {
                type master;
                file "/etc/bind/db.contoso.com";
        };
        

2. Populate the zone file:
    
        $TTL    604800
        @       IN      SOA     localhost. root.localhost. (
                                      1         ; Serial
                                      1         ; Refresh
                                      1         ; Retry
                                      1         ; Expire
                                      1 )       ; Negative Cache TTL
        ;
        @       IN      NS      localhost.
        @       IN      DNAME   mesos.yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo.dcos.directory.
        

## <a name="existing"></a>Using an existing zone

- To use an existing zone, add a DNAME record:
    
        @       IN      DNAME   mesos.yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo.dcos.directory.
        
    
    The `@` aliases the top level of the zone, for example `contoso.com`.

- To alias a high level domain, specify that value in the DNAME record. In this example, `foo` aliases `foo.contoso.com`:
    
        foo       IN      DNAME   mesos.yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo.dcos.directory.