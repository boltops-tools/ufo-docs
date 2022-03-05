---
title: Destroy App
---

Let's now destroy the app and clean up:

    ufo destroy

You will be prompted to confirm before deletion.

    $ ufo destroy
    You are about to destroy the demo-web-dev stack
    Are you sure you want to do this? (y/N)

Confirm to delete the resources:

    Are you sure you want to do this? (y/N) y
    Deleting stack with ECS resources: demo-web-dev
    Waiting for stack to complete
    10:03:46PM DELETE_IN_PROGRESS AWS::CloudFormation::Stack demo-web-dev User Initiated
    10:03:52PM DELETE_IN_PROGRESS AWS::ECS::Service EcsService
    ...
    Stack demo-web-dev deleted.
    Time took: 2m 38s

Let's double-check that the resources have been deleted:

    $ ufo ps
    No stack demo-web-dev found

Tip: If you want to delete without the prompt, you can use the `-y` option:

    ufo destroy -y

Next, we'll look at some next steps.
