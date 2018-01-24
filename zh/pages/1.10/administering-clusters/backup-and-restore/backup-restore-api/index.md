---
layout: layout.pug
navigationTitle: Backup and Restore API
title: Backup and Restore API
menuWeight: 10
excerpt: ""
enterprise: true
---
您可以使用备份和还原 API 来创建和还原群集的备份。

**Important:** See the [Limitations](/1.10/administering-clusters/backup-and-restore/#limitations) of backup and restore.

# 途径

通过使用以下路由, 可以通过每个主节点上的管理路由器代理对备份和还原 API 的访问:

    /system/v1/backup/v1
    

要确定群集的 URL, 请参阅 [ 群集访问 ](/1.10/api/access/)。

# 格式

备份和还原 API 请求和响应主体的格式为 JSON。

请求必须包括接受标头:

    Accept: application/json
    

响应包括内容类型标头:

    Content-Type: application/json
    

# 验证

所有备份和还原 API 路由都需要使用身份验证。

To authenticate API requests, see [Obtaining an authentication token](/1.10/security/ent/iam-api/#obtaining-an-authentication-token) and [Passing an authentication token](/1.10/security/ent/iam-api/#passing-an-authentication-token).

The Backup and Restore API also requires authorization via the following permissions:

| 资源 ID                                | 操作     |
| ------------------------------------ | ------ |
| `dcos:adminrouter:ops:system-backup` | `full` |

使用 ` dcos:superuser ` 权限的用户也可以访问所有路由。

要为您的帐户分配权限, 请参阅 [ 权限参考 ](/1.10/security/ent/perms-reference/)。

# API 引用

The Backup and Restore API allows you to manage backup and restore operations on your DC/OS cluster.

[swagger api='/1.10/api/backup-restore.yaml']