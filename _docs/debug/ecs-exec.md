---
title: Debugging Exec Into Container
nav_text: ECS Exec
category: debug
order: 3
---

The `ufo exec` command allows you to hop into a container and debug. It essentially calls [aws ecs execute-command](https://docs.aws.amazon.com/cli/latest/reference/ecs/execute-command.html).

## Setup: Overview

It requires some prerequisites. Mainly:

* The [Session Manager Plugin Installed](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)
* IAM Permissions: The ECS Task requires permission to call ssmmessages.

## Setup: IAM Permissions

Here's an example IAM Role Permisssion, you can add with UFO that grants access.

.ufo/resources/iam_roles/task_role.rb

```ruby
iam_policy("EcsExecuteCommand",
  Action: [
    "ssmmessages:CreateControlChannel",
    "ssmmessages:CreateDataChannel",
    "ssmmessages:OpenControlChannel",
    "ssmmessages:OpenDataChannel",
  ],
  Effect: "Allow",
  Resource: "*",
)
```

For more limited permissions, see: [Using IAM policies to limit access to ECS Exec
](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-exec.html#ecs-exec-best-practices-limit-access-execute-command)

Remember the IAM role we're working with here is the task role. See: [UFO Docs: Task Definition IAM Roles]({% link _docs/intro/iam/task.md %}).

Also, the [Amazon ECS Exec Checker](https://github.com/aws-containers/amazon-ecs-exec-checker) tool is helpful for double-checking permissions, including the ECS cluster and task definition.

## Setup: Session Manager on EC2 Instances

If you're using ECS EC2 where you have full control of the EC2 instances, you need to ensure that the EC2 instances have Session Manager properly set up. See: [Setting up Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html). To confirm that the EC2 instances are registered to the SSM service, you can use this command:

    aws ssm describe-instance-information

You can also check the SSM console.

## Setup: ECS Service EnableExecuteCommand

Make sure that `EnableExecuteCommand` is enabled on the ECS Service. UFO enables it by default. The reasoning is that this feature is just so helpful. Also, `heroku run` works right out of the gates. Albeit, heroku's version is slightly different since it starts up a new container, . Also, copilot also enables it by default for its version, `copilot svc exec`, to work right off the bat. Nevertheless, it is also possible to disable it. Example:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.exec.enabled = false # disabling but `ufo exec` and `aws ecs execute-command` wont work
end
```

## Error: Overview

Next, are some common errors with `aws ecs execute-command` that you may run into.

## Error: fork/exec /bin/bash

If you're getting this error

    ----------ERROR-------
    Unable to start command: Failed to start pty: fork/exec /bin/bash: no such file or directory

It means the `bash` shell is not available in the Docker container. Try a different shell. Example:

    ufo exec --command /bin/sh

You can also configure the default command ufo uses with

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.exec.command = "/bin/sh"
end
```

## Error: IAM Permissions

If you're seeing this error:

    An error occurred (TargetNotConnectedException) when calling the ExecuteCommand operation: The execute command failed due to an internal error. Try again later.

It's likely an IAM permission issue. See IAM Permissions above.

