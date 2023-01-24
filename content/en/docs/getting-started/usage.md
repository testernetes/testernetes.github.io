---
title: "CLI Usage"
linkTitle: "CLI Usage"
weight: 20
description: >-
     How to use the BDK CLI
---

## BDK CLI Usage

```
Behaviour Driven Kubernetes

Usage:
  bdk [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  help        Help about any command
  matchers    View matchers
  steps       View steps
  test        Run a test suite of feature files

Flags:
  -h, --help              help for bdk
  -p, --plugins strings   Additional plugin step definitions

Use "bdk [command] --help" for more information about a command.
```

### Test

```
Usage:
  bdk test [-f format] features... [flags]

Flags:
  -f, --format string   the format printer (default "simple")
  -h, --help            help for test

Global Flags:
  -p, --plugins strings   Additional plugin step definitions
```

Where features is a set of 1 or more feature files to test.
