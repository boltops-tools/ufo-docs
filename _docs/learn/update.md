---
title: Update App
---

Let's deploy

    ufo ship

You will be prompted to confirm before deployment.

    $ ufo ship
    Will deploy stack demo-web-dev (y/N)

Confirm to ship:

    Will deploy stack demo-web-dev (y/N) y
    Building Docker Image
    => docker build -t 536766270177.dkr.ecr.us-west-2.amazonaws.com/demo:ufo-2022-03-02T21-40-14-12dc6e0 -f Dockerfile .
    Docker Image built: 536766270177.dkr.ecr.us-west-2.amazonaws.com/demo:ufo-2022-03-02T21-40-14-12dc6e0
    Pushing Docker Image
    => docker push 536766270177.dkr.ecr.us-west-2.amazonaws.com/demo:ufo-2022-03-02T21-40-14-12dc6e0
    Task Definition built: .ufo/output/task_definition.json
    Parameters built:      .ufo/output/params.json
    Template built:        .ufo/output/template.yml
    Updating stack demo-web-dev
    Waiting for stack to complete
    09:40:20PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack demo-web-dev User Initiated
    09:40:35PM UPDATE_IN_PROGRESS AWS::ECS::Service EcsService
    ...
    09:42:34PM UPDATE_COMPLETE AWS::CloudFormation::Stack demo-web-dev
    Stack success status: UPDATE_COMPLETE
    Time took: 2m 17s
    Software shipped!
    $

## Check Changes

Let's first check the autoscaling changes with:

    ufo ps

You'll see `Max: 3` in the output like this:

    $ ufo ps
    Stack: demo-web-dev
    Service: demo-web-dev-EcsService-A8kB93gxbG9u
    Tasks: Running: 1 Desired: 1 Min: 1 Max: 3
    Application ELB: demo-we-Elb-3249Z2HSFMDA-1395285973.us-west-2.elb.amazonaws.com
    +----------------------------------+------+-----------------+---------------+---------+
    |             Task Id              | Name |     Release     |    Started    | Status  |
    +----------------------------------+------+-----------------+---------------+---------+
    | 67ad1eae238341cfbf32803648465d54 | web  | demo-web-dev:18 | 2 minutes ago | RUNNING |
    +----------------------------------+------+-----------------+---------------+---------+
    $


Let's also confirm that the task is using higher CPU and Memory limits with:

    aws ecs describe-tasks --cluster CLUSTER --tasks TASK

We'll filter the output with jq. Here's what it should look like:

    $ aws ecs describe-tasks --cluster dev --tasks 67ad1eae238341cfbf32803648465d54 \
        | jq '.tasks[].containers[] | {cpu: .cpu, memory: .memory}'
    {
      "cpu": "512",
      "memory": "512"
    }

We can see that both cpu and memory are set to 512.

Next, we'll delete the app.
