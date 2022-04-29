---
title: Failed ELB Health Checks Post Deployment
nav_text: Failed ELB Post Deploy
category: debug
order: 10
---

If you're seeing a `Task Stopped Reason: Task failed ELB health checks` error after an ECS deployment like this:

    Time took: 3m 54s
    Software shipped!
    Stack: demo-dev
    Service: demo-dev-EcsService-7pIXpAWS6VVR
    Tasks: Running: 1 Desired: 1 Min: 1 Max: 3
    Application ELB: demo-Elb-N2F2H887WJON-658045478.us-west-2.elb.amazonaws.com
    [
      {
        "Task": "029e357a8ff4490e8b7edb2702f38d50",
        "Name": "web",
        "Release": "demo-dev:8",
        "Started": "1 minutes ago",
        "Status": "RUNNING",
        "Notes": null
      },
      {
        "Task": "4a37407823cd4accb75170f6eda92887",
        "Name": "web",
        "Release": "demo-dev:8",
        "Started": "2 minutes ago",
        "Status": "STOPPED",
        "Notes": "Task Stopped Reason: Task failed ELB health checks in (target-group arn:aws:elasticloadbalancing:us-west-2:111111111111:targetgroup/demo-Targe-1GXHAH9WXSU9V/8e6d68f88cd88d12)."
      }
    ]

This might mean that the `health_check_interval_seconds` and `unhealthy_threshold_count` are configured too aggressively for your app. For example, a `settings unhealthy_threshold_count = 2` and `health_check_interval_seconds = 10` means a total of `20s` before the ELB will see the task as unhealthy.

ECS sends a soft kill signal to the Docker container during a deployment. If the app process doesn't fully stop the container after 30s, ECS sends a hard `kill -9` signal. However, `30s > 20s` so you may intermittently see the error reported. Essentially, the app hasn't fully finished cleaning up yet and the ELB detects it's unhealthy before it can.

In this case, you might want to increase the `unhealthy_threshold_count = 3`, so the ELB waits `30s` before seeing the task as unhealthy.