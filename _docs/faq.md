---
title: FAQ
---

**Q: Is AWS ECS Fargate supported?**

Yes, Fargate is supported.  To use ufo with Fargate, you will need to adjust the template in `.ufo/resources/task_defintions/web.yml` to a structure supported by Fargate.  There are 2 key items to adjust:

1. Notably, Fargate requires `requiresCompatibilities`, `networkMode`, and `executionRoleArn` attributes. The `cpu` and `memory` attributes also move outside of the `containerDefinitions` level to the top-level attributes.
2. The params that get sent to the `run_task` API methods.

**Q: Can I tell ufo to use specific docker build options?**

Yes, you can do this with the environment variable `UFO_DOCKER_BUILD_OPTIONS`.  Example:

```
$ UFO_DOCKER_BUILD_OPTIONS="--build-arg RAILS_ENV=production" ufo docker build
Building docker image with:
  docker build --build-arg RAILS_ENV=production -t org/repo:ufo-2018-05-19T11-52-16-6714713 -f Dockerfile .
...
Docker image org/repo:ufo-2018-05-19T11-52-16-6714713 built.  Took 2s.
```

---

**Q: What's the difference between ufo vs ecs-deploy?**

Some differences:

* ecs-deploy is implemented in bash
* ufo is implemented in ruby
* ecs-deploy is simpler, which could work better for you. It’s nice that it’s one bash script.

UFO does 3 things:

{% include intro/3-steps.md %}

Ecs-deploy is more designed to update existing an ECS service.

The main difference is that ecs-deploy downloads current ECS task definition and replaces the image attribute. ufo regenerates the task definition from code each deploy. If you’re making adjustments to the task definition with the ECS console as your flow, the ecs-deploy makes more sense. If you want to keep your task definition codified, than ufo makes more sense.

More differences:

* ufo creates the CloudWatch log group if doesn’t exist. It's a small thing but easy to forget.
* ufo has a concept of variables that get layered. This is how it handles multi environments with very similar task definitions with just a few differences.

Generally, ufo does more in scope, so we’re comparing apples to oranges here.

---

**Q: How can I use syslog instead of awslogs for logging?**

You can adjust `.ufo/resources/task_definitions/web.yml` to something like this:

```yaml
logConfiguration
  logDriver: syslog
```

Here's the specific aws docs section [Specifying a Log Configuration in your Task Definition](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html#specify-log-config)

Also, you might have to enable the log driver by adding the ECS_AVAILABLE_LOGGING_DRIVERS variable to your `/etc/ecs/ecs.config`. Relevant docs:

* [Using AWS Logs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html#enable_awslogs)
* [ECS Agent Install](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent-install.html)

