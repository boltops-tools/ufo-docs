---
title: Config Reference
nav_text: Reference
category: config
order: 88
---


## Required Options

The are only 2 required config options. The rest are optional. The required options:

    config.app            # App name. IE: demo Note: if UFO_APP is set then this is optional
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
{% include config/reference/ecs.md %}
{% include config/reference/elb.md %}
{% include config/reference/exec.md %}
layering.show | false | Shows used layers for both config and vars. Very useful for debugging layers. There are nuances with this option. It should be set in `.ufo/config.rb` and not be set dynamically. So only `true` or `false` values should be used. This is because config layers are processed so early that UFO parses the config file for this value internally.
{% include config/reference/logging.md %}
{% include config/reference/names.md %}
ps.format | auto | Default format of ps tasks output. Examples: auto csv table tab json. The auto format means table format is used if terminal is wide enough. If terminal is not wide enough, json format is used.
ps.hide_age | 5 | Age in minutes before hiding stopped tasks from `ufo ps`. Uses stopped_at and status of STOPPED.
ps.summary | true | Turns on or off the summary at the top of `ufo ps`.
{% include config/reference/secrets.md %}
{% include config/reference/state.md %}
ship.docker.quiet | false | Quiet docker output by writing output to `.ufo/log/docker.log`. It only affects `ufo ship` docker output. The `ufo docker build` command will still show output to the terminal.
{% include config/reference/vpc.md %}
