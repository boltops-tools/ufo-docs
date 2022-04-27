---
title: Debugging Container Instance Missing Attributes
nav_text: Missing Attributes
category: debug
order: 2
---

If you're getting this error:

    The closest matching container-instance is missing an attribute required by your task

This page may help.

## ecs-cli check-attributes

You can use this command to confirm that no attributes are missing:

    ecs-cli check-attributes --task-def TASK_DEF --container-instances CONTAINER_INSTANCES --cluster CLUSTER

Example:

    $ ecs-cli check-attributes --task-def demo-web:331 --container-instances 0c8b08621ec44444b07001f8eb02d771 --cluster dev
    Container Instance  Missing Attributes
    dev                 None

## Network Mode awsvpc and Private Subnets

If the `check-attributes` reports `None`, but you're still getting the "missing attributes" error. This might be because you're using `networkMode=awsvpc` but are deploying your ECS tasks to public subnets.  With `networkMode=awsvpc`, the ECS tasks must be deployed to private subnets.

See: [Task networking with the awsvpc network mode](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-networking-awsvpc.html)

> When hosting tasks that use the awsvpc network mode on Amazon EC2 Linux instances, your task ENIs are not given public IP addresses. To access the internet, tasks should be launched in a private subnet that is configured to use a NAT gateway.

Sadly, the "missing attributes" error doesn't tell you that. Also, found sometimes ECS task with awsvpc network more to public subnets appear to work. For example, you got from bridge mode to awsvpc mode and the subnet before and after were public subnets. However, it's may stop working. Since it doesn't seem to work inconsistently, suggest sticking to the AWS docs and running awsvpc ECS tasks in private subnets.
