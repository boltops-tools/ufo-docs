---
title: Debugging awsvpc ENI Limits
nav_text: ENI Limits
category: debug
order: 7
---

If you're seeing ECS tasks stuck in PENDING or PROVISIONING state for a very long time. Example:

    $ ufo ps
    +----------------------------------+------+-----------------+----------------+--------------+
    |               Task               | Name |     Release     |    Started     |    Status    |
    +----------------------------------+------+-----------------+----------------+--------------+
    | 057ff9b63756439295ce50f0934ae8fd | web  | demo-web-dev:23 | 44 seconds ago | RUNNING      |
    | 5c7cd3c5b8c14c8c9855692ca4e45e12 |      | demo-web-dev:23 | PENDING        | PROVISIONING |
    | 7ea868157f4444f5a73bf27a1885b82a |      | demo-web-dev:23 | PENDING        | PROVISIONING |
    +----------------------------------+------+-----------------+----------------+--------------+
    $

And eventually, usually, after 30 minutes, the ECS task fails to start. You'll see an error with `stopCode: TaskFailedToStart` and `stoppedReason: Task failed to start`. [AWS Docs: Stopped tasks error codes](https://docs.aws.amazon.com/AmazonECS/latest/userguide/stopped-task-error-codes.html). Here's also the aws cli command to debug:

    $ aws ecs describe-tasks --cluster dev --tasks 9420393263554a9681a1cd0ec31d02da | jq .
    {
      "tasks": [
        {
          "attachments": [],
          "capacityProviderName": "cp-ecs-dev",
          "clusterArn": "arn:aws:ecs:us-west-2:111111111111:cluster/dev",
          "containers": [],
          "cpu": "384",
          "createdAt": "2022-03-09T13:33:40.078000+00:00",
          "desiredStatus": "STOPPED",
          "enableExecuteCommand": false,
          "group": "service:demo-web-dev-EcsService-5x6vCgmTxCof",
          "healthStatus": "UNKNOWN",
          "lastStatus": "STOPPED",
          "launchType": "EC2",
          "memory": "256",
          "overrides": {
            "containerOverrides": [],
            "inferenceAcceleratorOverrides": []
          },
          "startedBy": "ecs-svc/5242710163844512662",
          "stopCode": "TaskFailedToStart",
          "stoppedAt": "2022-03-09T14:03:51.421000+00:00",
          "stoppedReason": "Task failed to start",
          "stoppingAt": "2022-03-09T14:03:51.421000+00:00",
          "tags": [],
          "taskArn": "arn:aws:ecs:us-west-2:111111111111:task/dev/9420393263554a9681a1cd0ec31d02da",
          "taskDefinitionArn": "arn:aws:ecs:us-west-2:111111111111:task-definition/demo-web-dev:23",
          "version": 2
        }
      ],
      "failures": []
    }

This can happen because there are not enough ENIs or Network Cards in the cluster for ECS to create and attach to. You're hitting the ENI limit. It can also happen if you have hit your ECS EC2 AutoScaling max size and there's not enough capacity for the ECS tasks.

Sadly, the error is pretty obtuse, and not every well surfaced. You have to dig all the way into stopped tasks, and then you see quite a useless message about "Task failed to start".

Even if you're using ECS EC2 and have access to the `/var/log/ecs` logs, it doesn't help. There are informational messages about successfully created network devices, not about failure.

    $ tail -f /var/log/ecs/*.log*
    ==> ecs-cni-eni-plugin.log.2022-03-09-01 <==
    2022-03-09T01:52:03Z [INFO] Found network device for the ENI (mac address=06:6f:1c:4a:40:9f): eth1
    2022-03-09T01:52:03Z [INFO] ENI 06:6f:1c:4a:40:9f (device name=eth1) has been assigned to the container's namespace

    ==> ecs-init.log <==
    2022-03-07T05:44:56Z [INFO] Starting Amazon Elastic Container Service Agent

    ==> ecs-volume-plugin.log <==
    level=info time=2022-03-07T05:44:23Z msg="Loading plugin state information"
    level=info time=2022-03-07T05:44:23Z msg="Starting volume plugin.."

    ==> ecs-agent.log <==
    level=info time=2022-03-09T13:34:17Z msg="TCS Websocket connection closed for a valid reason" module=handler.go
    level=info time=2022-03-09T13:34:17Z msg="Using cached DiscoverPollEndpoint. endpoint=https://ecs-a-5.us-west-2.amazonaws.com/ telemetryEndpoint=https://ecs-t-5.us-west-2.amazonaws.com/ containerInstanceARN=arn:aws:ecs:us-west-2:111111111111:container-instance/dev/0fa40593bbc740fb94680b9383e0f3df" module=client.go
    level=info time=2022-03-09T13:34:17Z msg="Establishing a Websocket connection to https://ecs-t-5.us-west-2.amazonaws.com/ws?agentHash=ed81f6b4&agentVersion=1.59.0&cluster=dev&containerInstance=arn%3Aaws%3Aecs%3Aus-west-2%3A111111111111%3Acontainer-instance%2Fdev%2F0fa40593bbc740fb94680b9383e0f3df&dockerVersion=20.10.7" module=client.go
    level=info time=2022-03-09T13:34:17Z msg="Connected to TCS endpoint" module=handler.go
    level=info time=2022-03-09T13:36:20Z msg="Managed task at steady state" taskARN="arn:aws:ecs:us-west-2:111111111111:task/dev/0445aac323b74cba933c6f3d8c767d4e" knownStatus="RUNNING"

Sometimes you may get a error that is more useful.  Example:

> Service demo-web-dev was unable to place a task because no container instance met all of its requirements.

And depending on the error ECS will surface a useful reason.

> Reason: No Container Instances were found in your cluster.

It depends on the error. Also see: [How do I resolve the "[AWS service] was unable to place a task because no container instance met all of its requirements" error in Amazon ECS?](https://aws.amazon.com/premiumsupport/knowledge-center/ecs-container-instance-requirement-error/)

If you need the awsvpc feature, the "Task failed to start" is just something to be aware of. Knowing what to look for can help.
