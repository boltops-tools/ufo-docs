---
title: ufo logs command
---

The ufo logs command will tail the logs of the ECS service if you are using the awslogs driver.

## Examples

    $ ufo logs
    Showing logs for stack: demo-web-dev log group: ecs/demo-dev and stream prefix: web
    2022-03-03 00:53:51 UTC 172.31.25.153 - - [03/Mar/2022:00:53:51 +0000] "GET / HTTP/1.1" 200 615 "-" "ELB-HealthChecker/2.0" "-"
    2022-03-03 00:53:51 UTC 172.31.37.4 - - [03/Mar/2022:00:53:51 +0000] "GET / HTTP/1.1" 200 615 "-" "ELB-HealthChecker/2.0" "-"

## Options

The logs follow and use the simple format without the log stream by default. Here's how to adjust those options:

    ufo logs --no-follow
    ufo logs --format detailed # to show stream too

More info: [ufo logs reference]({% link _reference/ufo-logs.md %})

## awslog driver

The generated Task Definition set up the awslogs driver. If you need it, here's the relevant section:

.ufo/resources/task_definitions/web.yml

```yaml
logConfiguration:
  logDriver: awslogs
  options:
    awslogs-group: "<%= @awslogs_group %>"
    awslogs-region: "<%= @awslogs_region || 'us-east-1' %>"
    awslogs-stream-prefix: "<%= @awslogs_stream_prefix %>"
```

And section of the variables file:

.ufo/vars/base.rb

```ruby
@awslogs_group = ["ecs/#{UFO.app}", Ufo.env, Ufo.env_extra].compact.join('-')
@awslogs_stream_prefix = role
@awslogs_region = aws_region
```
