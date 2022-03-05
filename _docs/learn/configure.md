---
title: Configure AWS
---

Configure AWS so UFO can connect to it. The recommended way is to:

1. set up the `~/.aws/config` and `~/.aws/credentials` files
2. set up your `AWS_PROFILE` environment variable.

## Example

~/.aws/config

    [profile dev]
    output = json
    region = us-west-2

~/.aws/credentials

    [dev]
    aws_access_key_id = REPLACE_ME
    aws_secret_access_key = REPLACE_ME

In your `~/.bashrc` or `~/.profile`, use this line to set the `AWS_PROFILE` environment variable:

    export AWS_PROFILE=dev

## Test AWS Setup

Here are some useful commands to test that the AWS CLI is working:

    aws sts get-caller-identity
    aws s3 ls

## Resources

* [AWS CLI Docs](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

Next, we'll provide some links to setting up an ECS cluster.
