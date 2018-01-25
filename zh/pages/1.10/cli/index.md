---
layout: layout.pug
navigationTitle: CLI
title: CLI
menuWeight: 50
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

Dc/os 命令行界面 (dc/os CLI) 是管理群集节点、安装和管理包、检查群集状态以及管理服务和任务的实用程序。

DC / OS 1.10.0需要DC / OS CLI 0.5.x.

要列出可用命令，请运行不带参数的` dcos `：

```bash
dcos

Command line utility for the Mesosphere Datacenter Operating
System (DC/OS). The Mesosphere DC/OS is a distributed operating
system built around Apache Mesos. This utility provides tools
for easy management of a DC/OS installation.

Available DC/OS commands:

    auth            Authenticate to DC/OS cluster
    cluster         Manage connections to DC/OS clusters
    config          Manage the DC/OS configuration file
    experimental    Experimental commands. These commands are under development and are subject to change
    help            Display help information about DC/OS
    job             Deploy and manage jobs in DC/OS
    marathon        Deploy and manage applications to DC/OS
    node            Administer and manage DC/OS cluster nodes
    package         Install and manage DC/OS software packages
    service         Manage DC/OS services
    task            Manage DC/OS tasks

Get detailed command description with `dcos <command> --help`.
```

# 显示DC / OS CLI版本

要显示 DC/OS CLI 版本, 请运行:

    dcos --version
    

<a name="configuration-files"></a>

# DC / OS CLI版本和配置文件

DC / OS CLI 0.4.x和0.5.x使用不同的结构来配置文件的位置。

DC/OS CLI 0.4.x 有一个配置文件, 默认情况下存储在 ` dcos/dcos. toml ` 中。 在DC / OS CLI 0.4.x中，您可以使用环境变量[ ` DCOS_CONFIG ` ](#dcos-config)来选择更改配置文件的位置。

DC/OS CLI 0.5.x has a configuration file for each connected cluster, which by default are stored in `~/.dcos/clusters/<cluster_id>/dcos.toml`. In DC/OS CLI 0.5.x you can optionally change the base portion (`~/.dcos`) of the configuration directory using the [`DCOS_DIR`](#dcos-dir) environment variable.

**Note:** - Updating to the DC/OS CLI 0.5.x and running any CLI command triggers conversion from the old to the new configuration structure. - After you call `dcos cluster setup`, (or after conversion has occurred), if you attempt to update the cluster configuration using a `dcos config set` command, the command prints a warning message saying the command is deprecated and cluster configuration state may now be corrupted.

# Environment variables

The DC/OS CLI supports the following environment variables, which can be set dynamically.

<a name="dcos-cluster"></a>

#### `DCOS_CLUSTER` (DC/OS CLI O.5.x and higher only)

The [attached](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-attach/) cluster. To set the attached cluster, set the variable with the command:

```bash
export DCOS_CLUSTER=<cluster_name>
```

<a name="dcos-config"></a>

#### `DCOS_CONFIG` (DC/OS CLI O.4.x only)

The path to a DC/OS configuration file. If you put the DC/OS configuration file in `/home/jdoe/config/dcos.toml`, set the variable with the command:

```bash
export DCOS_CONFIG=/home/jdoe/config/dcos.toml
```

If you have the `DCOS_CONFIG` environment variable configured:

* After conversion to the [new configuration structure](#configuration-files), `DCOS_CONFIG` is no longer honored.
* Before you call `dcos cluster setup`, you can change the configuration pointed to by `DCOS_CONFIG` using `dcos config set`. This command prints a warning message saying the command is deprecated and recommends using `dcos cluster setup`.

<a name="dcos-dir"></a>

#### `DCOS_DIR` (DC/OS CLI O.5.x and higher only)

The path to a DC/OS configuration directory. If you want the DC/OS configuration directory to be `/home/jdoe/config`, set the variable with the command:

```bash
export DCOS_DIR=/home/jdoe/config
```

1. Optionally set `DCOS_DIR` and run `dcos cluster setup` command.
    
        export DCOS_DIR=<path/to/config_dir> (optional, default when not set is ~/.dcos)
        dcos cluster setup <url>
        
    
    This setting generates and updates per cluster configuration under `$DCOS_DIR/clusters/<cluster_id>`. Sets newly set up cluster as the attached one.

<a name="dcos-ssl-verify"></a>

#### `DCOS_SSL_VERIFY`

Indicates whether to verify SSL certificates or set the path to the SSL certificates. You must set this variable manually. Setting this environment variable is equivalent to setting the `dcos config set core.ssl_verify` option in the DC/OS configuration [file](#configuration-files). For example, to indicate that you want to set the path to SSL certificates:

```bash
export DCOS_SSL_VERIFY=false
```

<a name="dcos-log-level"></a>

#### `DCOS_LOG_LEVEL`

Prints log messages to stderr at or above the level indicated. This is equivalent to the `--log-level` command-line option. The severity levels are:

* **debug** Prints all messages to stderr, including informational, warning, error, and critical.
* **info** Prints informational, warning, error, and critical messages to stderr.
* **warning** Prints warning, error, and critical messages to stderr.
* **error** Prints error and critical messages to stderr.
* **critical** Prints only critical messages to stderr.

For example, to set the log level to warning:

```bash
export DCOS_LOG_LEVEL=warning
```

<a name="dcos-debug"></a>

#### `DCOS_DEBUG`

Indicates whether to print additional debug messages to `stdout`. By default this is set to `false`. For example:

```bash
export DCOS_DEBUG=true
```