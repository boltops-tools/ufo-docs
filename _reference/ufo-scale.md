---
title: ufo scale
reference: true
---

## Usage

    ufo scale

## Description

Scale the ECS service

Ufo provides a command to scale up and down an ECS service quickly. However, the scaling configuration is only a temporary update until the next `ufo ship`.

    ufo scale --desired 2

If you've configured ECS Service AutoScaling with ufo then you can also update the Scaling configuration.

    ufo scale --min 1 --max 2


## Options

```
[--desired=N]  # Desired count
[--min=N]      # Minimum capacity
[--max=N]      # Maximum capacity
```

