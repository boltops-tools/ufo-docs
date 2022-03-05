---
title: Task Definition IAM Roles
nav_text: Task IAM
category: intro
order: 9
---

## What are ECS IAM Roles?

For ECS Task Definitions, you can assign it 2 IAM roles:

1. **taskRoleArn**: ECS Task Docker container. [AWS Docs: IAM Roles for Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)
2. **executionRoleArn**: EC2 Instance ecs-agent. [AWS Docs: Amazon ECS task execution IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html)

It's usually defined like so:

.ufo/resources/task_definitions/web.yml

```yaml
family: <%= @family %>
taskRoleArn: "arn:aws:iam::112233445566:role/name"
executionRoleArn: "arn:aws:iam::112233445566:role/name"
```

Here's a table that explains the difference between the 2 IAM roles.

Name | Purpose
--- | ---
taskRoleArn | This is the role that the **ECS task** itself uses. So this is what IAM permissions your application has access to. Think about it as the "container role".
executionRoleArn | This is the role that **ecs-agent** on the **EC2 instance** host uses. Examples are allowing the EC2 instance to pull from ECR. Or grab sensitive data from Secrets Manager or AWS Systems Manager Parameter Store. Think about it as the "host role".

Note: This is diffrent from the [Deploy IAM Permissions]({% link _docs/intro/iam/deploy.md %}).

## How to Assign IAM Roles with UFO

You can assign an IAM role to the ECS Task definition in ways:

1. IAM Role with Code (UFO Managed)
2. Precreated IAM Role

## IAM Role with Code (UFO Managed)

UFO can automatically create the IAM and assign it to the task definition. You create these files so UFO will know to create and manage the IAM roles. UFO adds these IAM Role resources to the CloudFormation template.

    .ufo/resources/iam_roles/execution_role.rb
    .ufo/resources/iam_roles/task_role.rb

### Example 1

You then use a DSL to create the IAM roles. Here are examples:

.ufo/resources/iam_roles/execution_role.rb

```ruby
managed_iam_policy("AmazonSSMReadOnlyAccess")
managed_iam_policy("SecretsManagerReadWrite")
managed_iam_policy("service-role/AmazonECSTaskExecutionRolePolicy")
```

.ufo/resources/iam_roles/task_role.rb

```ruby
iam_policy("AmazonS3ReadOnlyAccess",
  Action: [
    "s3:Get*",
    "s3:List*"
  ],
  Effect: "Allow",
  Resource: "*"
)
iam_policy("CloudwatchWrite",
  Action: [
    "cloudwatch:PutMetricData",
  ],
  Effect: "Allow",
  Resource: "*"
)
```

### Example 2

You can use the `managed_iam_policy` and `iam_policy` together. You can also group multiple statements in the `iam_policy` declaration.

.ufo/resources/iam_roles/task_role.rb

```ruby
managed_iam_policy("AmazonSSMManagedInstanceCore")

iam_policy("custom-policy", [
  {
    Action: "ecs:UpdateContainerInstancesState",
    Resource: "*",
    Effect: "Allow"
  },
  {
    Action: "sns:Publish",
    Resource: "*",
    Effect: "Allow"
  }
])
```

## Pre-Created IAM Role

You can also assign the task definition `executionRoleArn` with pre-created IAM roles. It looks something like this:

.ufo/resources/task_definitions/web.yml

```yaml
family: <%= @family %>
taskRoleArn: "arn:aws:iam::112233445566:role/pre-created-iam-role"
executionRoleArn: "arn:aws:iam::112233445566:role/pre-created-iam-role"
```

## App Roles Lookup Paths

If you're using UFO in a [central manner]({% link _docs/patterns/central-deployer.md %}), you can have app specific IAM roles. UFO looks up the path to the role to use based on `UFO_APP`. It works similar to `LOAD_PATH`.

A concrete example:

    .ufo/resources/iam_roles/:UFO_APP/task_role.rb # highest precedence
    .ufo/resources/iam_roles/task_role.rb

A concrete example with `UFO_APP=app1`:

    .ufo/resources/iam_roles/app1/task_role.rb
    .ufo/resources/iam_roles/task_role.rb
