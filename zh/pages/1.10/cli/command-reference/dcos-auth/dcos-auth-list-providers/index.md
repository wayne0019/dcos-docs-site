---
layout: layout.pug
navigationTitle: dcos auth list-providers
title: dcos auth list-providers
menuWeight: 1
excerpt: ""
enterprise: true
---
# 描述

List configured authentication providers for your DC/OS cluster. For more information, see the service accounts [documentation](/1.10/security/ent/service-auth/).

# 统计

```bash
dcos auth list-providers [OPTION]
```

# 选项

| 姓名、速记    | 默认 | 描述                                                         |
| -------- | -- | ---------------------------------------------------------- |
| `--json` |    | Specify a JSON-formatted list of authentication providers. |

# Parent command

| 命令                                                  | 描述                                |
| --------------------------------------------------- | --------------------------------- |
| [dcos auth](/1.10/cli/command-reference/dcos-auth/) | Manage DC/OS identity and access. |

# 例子

## List available providers

In this example, the available DC/OS authentication providers are listed.

```bash
dcos auth list-providers
```

输出会是这样：

```bash
PROVIDER ID    AUTHENTICATION TYPE                                                               
dcos-services  Authenticate using a DC/OS service user account (using username and private key)  
dcos-users     Authenticate using a standard DC/OS user account (using username and password)   
```