---
layout: layout.pug
navigationTitle: dcos auth login
title: dcos auth login
menuWeight: 2
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

验证到DC / OS。 [ dcos集群设置](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-setup)命令也运行` dcos auth login `。   上下文| 编辑上下文

# 统计

```bash
dcos auth login [OPTION]
```

# 选项

| 姓名、速记                                   | 默认 | 描述                                                                                                                       |
| --------------------------------------- | -- | ------------------------------------------------------------------------------------------------------------------------ |
| `--ca-certs=<ca-certs>`           |    | [enterprise type="inline" size="small" /] The path to a list of trusted CAs to verify requests against.                  |
| `--insecure`                            |    | 允许请求绕过SSL证书验证。 类似于` dcos config set core.ssl_verify = False`                                                             |
| `--no-check`                            |    | [enterprise type="inline" size="small" /] Do not check the CA certificate downloaded from the cluster. This is insecure. |
| `--password-env=<password_env>`   |    | 包含登录密码的环境变量的名称。                                                                                                          |
| `--password-file=<password_file>` |    | The path to a file that contains the password for login.                                                                 |
| `--password=<password>`           |    | The password for login. This is insecure.                                                                                |
| `--private-key=<key_path>`        |    | The path to a file that contains the private key.                                                                        |
| `--provider=<provider_id>`        |    | [enterprise type="inline" size="small" /] The authentication provider to use for login.                                  |
| `--username=<username>`           |    | The username for login.                                                                                                  |

## SSL options

If you do not specify one of the SSL options `--insecure`, `--no-check`, or `--ca-certs`, the CA certificate is downloaded from the cluster and a sha256 fingerprint of the certificate is presented to you for verification.

# Parent command

| Command                                             | Description                       |
| --------------------------------------------------- | --------------------------------- |
| [dcos auth](/1.10/cli/command-reference/dcos-auth/) | Manage DC/OS identity and access. |

# Examples