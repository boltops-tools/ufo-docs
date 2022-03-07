---
title: Minimal Deploy IAM Policy
nav_text: Deploy IAM
category: intro
order: 8
---

The IAM user you use to run the `ufo ship` command needs a minimal set of IAM policies to deploy to ECS.

Note: This is different than the [Task Definition IAM Roles]({% link _docs/intro/iam/task.md %}).

## Minimal IAM Permissions

Here is a table of the baseline services needed:

Service | Description
--- | ---
ApplicationAutoScaling | To create ECS Service AutoScaling policy and scalable target.
CloudFormation | To create the CloudFormation stack that creates most of the AWS resources that UFO creates, like ECS service and the ELB.
EC2 | To describe subnets associated with VPC. Used to configure subnets to use for ECS tasks and ELBs.
ECR | To pull and push to the ECR registry.  If you're using DockerHub, this permission is not required.
ECS | To create ECS service, task definitions, etc.
ElasticloadBalancing | To create the ELB and related load balancing resources like Listeners and Target Groups.
ElasticloadBalancingV2 | To create the ELB and related load balancing resources like Listeners and Target Groups.
IAM | To create ECS Task Role and Execution Role. See: [ECS IAM Roles]({% link _docs/intro/iam/task.md %}).
Logs | To write to CloudWatch Logs.
Route53 | To create vanity DNS endpoint when using [Route53 setting]({% link _docs/features/dns-route53-support.md %}).

## Instructions

It is recommended that you create an IAM group and associate it with the IAM users that need access to use `ufo ship`.  Here are starter instructions and a policy that you can tailor for your needs:

### Commands Summary

Here's a summary of the commands:

    aws iam create-group --group-name Ufo
    cat << 'EOF' > /tmp/ufo-policy.json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "application-autoscaling:*",
                    "cloudformation:*",
                    "ec2:*",
                    "ecr:*",
                    "ecs:*",
                    "elasticloadbalancing:*",
                    "elasticloadbalancingv2:*",
                    "iam:*",
                    "logs:*",
                    "route53:*",
                ],
                "Resource": "*",
                "Effect": "Allow"
            },
            {
                "Action": [
                    "iam:PassRole"
                ],
                "Effect": "Allow",
                "Resource": "*",
                "Condition": {
                    "StringLike": {
                        "iam:PassedToService": [
                            "ecs-tasks.amazonaws.com"
                        ]
                    }
                }
            }
        ]
    }
    EOF
    aws iam put-group-policy --group-name Ufo --policy-name UFOPolicy --policy-document file:///tmp/ufo-policy.json

Then create a user and add the user to IAM group. Here's an example:

    aws iam create-user --user-name tung
    aws iam add-user-to-group --user-name tung --group-name Ufo

## ECS Task IAM Policy vs User Deploy IAM Policy

This page refers to your **user** IAM policy used when running `ufo ship`. These are different from the IAM Policies associated with ECS Task.  For those IAM policies refer to [IAM Roles for Tasks
](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html).
