---
title: How UFO Works
category: intro
order: 1
---

UFO first builds the Docker image. Then it builds a CloudFormation template. Lastly, it calls CloudFormation to deploy your app and create the necessary ECS resources. It does these 3 steps:

{% include intro/3-steps.md %}

In fact, you can use UFO to build the files first, and then run `aws cloudformation create-stack` directly. Example:

    ufo build

The `ufo build` command builds the Docker image using your `Dockerfile` and builds the CloudFormation template.

    $ ufo build
    Building Docker Image
    => docker build -t 111111111111.dkr.ecr.us-west-2.amazonaws.com/demo:ufo-2022-03-02T22-50-56-12dc6e0 -f Dockerfile .
    => docker push 111111111111.dkr.ecr.us-west-2.amazonaws.com/demo:ufo-2022-03-02T22-50-56-12dc6e0
    Task Definition built: .ufo/output/task_definition.json
    Parameters built:      .ufo/output/params.json
    Template built:        .ufo/output/template.yml
    $

You can even run using the built params.json and template.yml to create the stack directly.

    aws cloudformation create-stack \
        --stack-name demo-web-dev \
        --template-body file://template.yml \
        --parameters file://params.json \
        --capabilities CAPABILITY_NAMED_IAM

UFO automates the process with:

    ufo ship
