---
layout: layout.pug
navigationTitle: dcos experimental package build
title: dcos experimental package build
menuWeight: 1
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

在本地构建一个包, 以将其添加到 DC/OS 或与宇宙共享。

# 统计

```bash
dcos experimental package build <build-definition> [OPTION]
```

# Options

| Name, shorthand                               | Default                   | Description                                            |
| --------------------------------------------- | ------------------------- | ------------------------------------------------------ |
| `--json`                                      |                           | JSON-formatted data.                                   |
| `--output-directory=<output-directory>` | current working directory | Path to the directory where the data should be stored. |

# Positional arguments

| Name, shorthand            | Default | Description                               |
| -------------------------- | ------- | ----------------------------------------- |
| `<build-definition>` |         | Path to a DC/OS package build definition. |

# Parent command

| Command                                                             | Description                                                   |
| ------------------------------------------------------------------- | ------------------------------------------------------------- |
| [dcos experimental](/1.10/cli/command-reference/dcos-experimental/) | Manage commands that under development and subject to change. |