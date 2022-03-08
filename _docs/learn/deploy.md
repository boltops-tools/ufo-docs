---
title: Deploy App
---

Let's deploy

    ufo ship

You will be prompted to confirm before deployment.

    $ ufo ship
    Will deploy stack demo-web-dev (y/N) y

Confirm to ship:

    Will deploy stack demo-web-dev (y/N) y
    Building Docker Image
    => docker build -t 111111111111.dkr.ecr.us-west-2.amazonaws.com/demo:ufo-2022-03-02T20-32-33-12dc6e0 -f Dockerfile .
    Docker Image built: 111111111111.dkr.ecr.us-west-2.amazonaws.com/demo:ufo-2022-03-02T20-32-33-12dc6e0
    Pushing Docker Image
    => docker push 111111111111.dkr.ecr.us-west-2.amazonaws.com/demo:ufo-2022-03-02T20-32-33-12dc6e0
    Task Definition built: .ufo/output/task_definition.json
    Parameters built:      .ufo/output/params.json
    Template built:        .ufo/output/template.yml
    Creating stack demo-web-dev
    Waiting for stack to complete
    08:32:39PM CREATE_IN_PROGRESS AWS::CloudFormation::Stack demo-web-dev User Initiated
    08:34:32PM CREATE_IN_PROGRESS AWS::ECS::Service EcsService
    ...
    08:35:08PM CREATE_COMPLETE AWS::CloudFormation::Stack demo-web-dev
    Stack success status: CREATE_COMPLETE
    Time took: 2m 32s
    Software shipped!
    $

**What did UFO do?**

{% include intro/3-steps.md %}

Tip: If you want to deploy without the prompt, you can use the `-y` option:

    ufo ship -y

## Check App

Let's check the app:

    ufo ps

Example output:

    $ ufo ps
    Stack: demo-web-dev
    Service: demo-web-dev-EcsService-A8kB93gxbG9u
    Tasks: Running: 1 Desired: 1 Min: 1 Max: 2
    Application ELB: demo-we-Elb-3249Z2HSFMDA-1395285973.us-west-2.elb.amazonaws.com
    +----------------------------------+------+-----------------+---------------+---------+
    |             Task Id              | Name |     Release     |    Started    | Status  |
    +----------------------------------+------+-----------------+---------------+---------+
    | 5c965f02cd5b475c9659a60877775267 | web  | demo-web-dev:17 | 3 minutes ago | RUNNING |
    +----------------------------------+------+-----------------+---------------+---------+
    $

We can see that 1 ECS task is running. Let's curl the ELB endpoint.

    $ curl -s demo-we-Elb-3249Z2HSFMDA-1395285973.us-west-2.elb.amazonaws.com | grep title
    <title>Welcome to nginx!</title>

It returns the Nginx welcome page.

**Tips:**

* You can turn off the summary at the top of `ufo ps` with `config.ps.summary = false`. Here are the [Config Docs]({% link _docs/config.md %}).
* By default, the docker output is written to a log. If you have a docker build process that's relatively quick, you may want to enable `config.ship.docker.quiet = true`. This writes the Docker output to a file, keeping the ship output pretty clean. 
* The `ufo ps` command is also called at the end of `ufo ship` for your convenience.

Next, we'll make a change.
