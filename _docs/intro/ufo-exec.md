---
title: Ufo Exec Into Container
nav_text: Ufo Exec
category: intro
order: 6
---

The `ufo exec` command allows you to hop into a container and debug. It essentially calls [aws ecs execute-command](https://docs.aws.amazon.com/cli/latest/reference/ecs/execute-command.html).

## Demo

Here's a demo. First, list the ECS Tasks or containers and confirm that there are running containers.

    $ ufo ps
    +----------------------------------+------+-----------------+---------------+---------+
    |             Task Id              | Name |     Release     |    Started    | Status  |
    +----------------------------------+------+-----------------+---------------+---------+
    | d5e7b8e238154e25b07ef5bd66e20abf | web  | demo-web-dev:25 | 3 minutes ago | RUNNING |
    +----------------------------------+------+-----------------+---------------+---------+

To hop into the container, it's simple:

    $ ufo exec
    => aws ecs execute-command --cluster dev --task d5e7b8e238154e25b07ef5bd66e20abf --container web --interactive --command /bin/bash
    Starting session with SessionId: ecs-execute-command-0412e0b1acbfd230e
    root@62e996573a7a:/# grep ID /etc/os-release
    VERSION_ID="11"
    ID=debian
    root@62e996573a7a:/#

The default command is `/bin/bash`, if that shell is not available try:

    $ ufo exec --command /bin/sh
    # grep ID /etc/os-release
    VERSION_ID="11"
    ID=debian
    #

You can also configure the default command with `config.exec.command`. Example:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.exec.command = "/bin/sh"
end
```

## Prerequisites for exec

To use the exec feature, some prerequisites must be set up. More info: [Debugging with Exec]({% link _docs/debug/ecs-exec.md %})

{% include config/reference/header.md %}
{% include config/reference/exec.md %}
