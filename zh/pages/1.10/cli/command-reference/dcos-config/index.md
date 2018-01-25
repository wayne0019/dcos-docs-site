---
layout: layout.pug
navigationTitle: dcos config
title: dcos config
menuWeight: 2
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

此命令管理运行 [ dcos cluster setup ](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-setup) 时创建的 DC/OS 配置文件。 配置文件位于 `~/.dcos/clusters/<cluster_id>/dcos.toml`. 如果未更改任何配置属性, 则在运行 ` dcos config show` 时应看到此输出:

    cluster.name <cluster_name>
    core.dcos_acs_token ********
    core.dcos_url <cluster_url>
    core.ssl_verify `true` or `false`
    

## 环境变量

配置属性具有相应的环境变量。 如果属性位于 `core` 部分 (ex。 `core.foo`), it corresponds to the environment variable `DCOS_FOO`. All other properties (ex. `foo.bar`) correspond to the environment variable `DCOS_FOO_BAR`.

Environment variables take precedence over corresponding configuration property.

# Usage

```bash
dcos config
```

# Options

| Name, shorthand | Default | Description                                   |
| --------------- | ------- | --------------------------------------------- |
| `--help, h`     |         | Print usage.                                  |
| `--info`        |         | Print a short description of this subcommand. |
| `--version, v`  |         | Print version information.                    |

# Child commands

| Command                                                                               | Description                                    |
| ------------------------------------------------------------------------------------- | ---------------------------------------------- |
| [dcos config set](/1.10/cli/command-reference/dcos-config/dcos-config-set/)           | Add or set a DC/OS configuration property.     |
| [dcos config show](/1.10/cli/command-reference/dcos-config/dcos-config-show/)         | Print the DC/OS configuration file contents.   |
| [dcos config unset](/1.10/cli/command-reference/dcos-config/dcos-config-unset/)       | Remove a property from the configuration file. |
| [dcos config validate](/1.10/cli/command-reference/dcos-config/dcos-config-validate/) | Validate changes to the configuration file.    |