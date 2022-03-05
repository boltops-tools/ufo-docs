---
title: ufo destroy
reference: true
---

## Usage

    ufo destroy

## Description

Destroy the ECS service.

## Examples

Deletes the CloudFormation stack and destroys the ECS Service resources.

    ufo destroy

If you would like to bypass the prompt, you can use the `-y` option.

    ufo destroy -y


## Options

```
y, [--yes], [--no-yes]     # By pass are you sure prompt.
    [--wait], [--no-wait]  # Wait for completion
                           # Default: true
```

