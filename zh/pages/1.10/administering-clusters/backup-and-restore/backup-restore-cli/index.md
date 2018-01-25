---
layout: layout.pug
navigationTitle: 备份和还原
title: 备份和还原
menuWeight: 0
excerpt: ""
enterprise: true
---
# 基础要求

- DC / OS企业集群。
- The [DC/OS CLI](/1.10/cli/install/) installed.
- The [DC/OS Enterprise CLI](/1.10/cli/enterprise-cli/) installed.

**Important:** See the [Limitations](/1.10/administering-clusters/backup-and-restore/#limitations) of backup and restore.

# 备份一个群集

备份存储在主节点的本地文件系统上。 Backup state is maintained by a service running in the cluster and backup/restore operations are initiated by hitting this service directly.

1. Create a backup and assign it a meaningful label. The label has the following restrictions:
    
    - It must be between 3 and 25 characters in length.
    - It cannot start with `..`.
    - It must be composed of the following characters: [A-Za-z0-9_.-].
    ```bash
dcos backup create --label=<backup-label>
```

2. Verify your backup has been created.
    
    ```bash
dcos backup list
```

Or use the following command to refine your search results to the label you used when you created the backup.

```bash
dcos backup list [label]
```

The backup will initially transition into the `STATUS_BACKING_UP` state, and should eventually arrive at `STATUS_READY`. If something goes wrong, it will show a state of `STATUS_ERROR`. Use `dcos backup show <backup-id>` to inspect why Marathon errored out during the course of the backup.

3. Use the ID produced by `dcos backup list` to refer to your backup in subsequent commands. A backup ID will resemble `<backup-label>-ea6b49f5-79a8-4767-ae78-3f874c90e3da`.

# 删除备份

1. Delete an unneeded backup.
    
    ```bash
dcos backup delete <backup-id>
```

# Restore a cluster

1. List the available backups, choose the backup you want to restore to, and make a note of the backup ID.
    
    ```bash
dcos backup list
```

2. Restore from the selected backup.
    
    ```bash
dcos backup restore <backup-id>
```

3. Monitor the status of the restore operation.
    
    ```bash
dcos backup show <backup-id>
```

The `restores.component_status.marathon` parameter of the JSON output will show `STATUS_RESTORING`, and then `STATUS_READY`.