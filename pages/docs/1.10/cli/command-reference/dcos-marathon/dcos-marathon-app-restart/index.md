---
layout: layout.pug
title: dcos marathon app restart
menuWeight: 5
---

# Description
Restart an application.

# Usage

```bash
dcos marathon app restart <app-id> [OPTION]
```

# Options

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `--force`   |             | Disable checks in Marathon during updates. |

# Positional arguments

| Name, shorthand | Default | Description |
|---------|-------------|-------------|
| `<app-id>`   |             |  The application ID. |

# Parent command

| Command | Description |
|---------|-------------|
| [dcos marathon](/docs/1.10/cli/command-reference/dcos-marathon/) | Deploy and manage applications to DC/OS. |

<!-- # Examples -->