---
title: Config Reference
nav_text: Reference
category: config
order: 88
---


## Required Options

The are only 2 required config options. The rest are optional. The required options:

    config.app            # App name. IE: demo Note: if UFO_ENV is set then this is optional
    config.docker.repo    # Docker Repo to push Docker image to

The vpc settings are notable. It's how you configure which VPC you want the ECS Service to use.

## Notable Env Vars

The UFO_APP, UFO_ENV, UFO_ROLE are worth noting. When set, they will take highest precedence. They are all optional.  The default values are:

    UFO_APP=nil # config.app is usually used
    UFO_ENV=dev
    UFO_ROLE=web

{% include config/reference/header.md %}
app | nil | The app name. Example: `demo`. This is normally required. Unless `UFO_APP` is set.
{% include config/reference/autoscaling.md %}
cfn.disable_rollback | false | Whether or not to disable rollback
cfn.notification_arns | nil | CloudFormation stack notification.
cfn.tags | nil | Hash of tags. IE: {Name: "value"}
{% include config/reference/docker.md %}
ecs.cluster | :ENV | Notice that the default is a pattern. By default, the `:ENV` pattern is expanded as the value of env var `UFO_ENV=dev`. So, by convention, the ECS cluster that ufo deploys to matches the `UFO_ENV`. If `UFO=prod`, then `ufo ship` deploys to the `prod` ECS cluster. This is option overrides this convention.
ecs.desired_count | nil | Only respected when `autoscaling.enabled = false`.
ecs.scheduling_strategy | nil | ECS Scheduling Strategy. IE: REPLICA or DAEMON
{% include config/reference/elb.md %}
{% include config/reference/exec.md %}
log.root | The root folder where logs are written to. | log
logger | Logger instance to use. | Logger.new($stderr)
logger.formatter | Logger Formatter to use. See [Formatter](https://ruby-doc.org/stdlib-2.7.1/libdoc/logger/rdoc/Logger/Formatter.html) for interface. | [Ufo::Logger::Formatter](https://github.com/boltops-tools/ufo/blob/master/lib/ufo/logger/formatter.rb)
logger.level | Logger level | info
logs.filter_pattern | nil | Default filter pattern to use. The CLI option overrides this setting. Example: `config.logs.filter_pattern = '- "HealthChecker"'`. Note the `-` minus sign rejects patterns. See: [AWS Docs: CloudWatch Logs Filter and Syntax Pattern](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html).
{% include config/reference/names.md %}
ps.format | auto | Default format of ps tasks output. Examples: auto csv table tab json. The auto format means table format is used if terminal is wide enough. If terminal is not wide enough, json format is used.
ps.hide_age | 5 | Age in minutes before hiding from `ufo ps`.
ps.summary | true | Turns on or off the summary at the top of `ufo ps`.
scale.warning | true | Disables the scale warning message about how manual adjustments should be codified for them to be kept.
secrets.pattern.secretsmanager | :APP-:ENV-:SECRET_NAME | Pattern used for secretsmanager secrets. It's expanded like so `:APP-:ENV-:SECRET_NAME` => `demo-dev-DB_PASS`
secrets.pattern.ssm | :APP/:ENV/:SECRET_NAME | Pattern use for ssm parameter store. It's expanded like so `:APP/:ENV/:SECRET_NAME` => `demo/dev/DB_PASS`
secrets.provider | ssm | Default provider for conventional expansion. Examples: ssm or secretsmanager
ship.docker.quiet | true | Quiet docker output by writing output to `.ufo/log/docker.log`. It only affects `ufo ship` docker output. The `ufo docker build` command will still show output to the terminal.
{% include config/reference/vpc.md %}
