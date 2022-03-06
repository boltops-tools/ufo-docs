---
title: ECS Secrets Pros and Cons
nav_text: Secrets Pros and Cons
category: more
order: 3
---

## What are Secrets?

[ECS supports injecting secrets or sensitive data](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data.html) into the environment as variables.

We'll go over some of the pros and cons of using secrets.

## Pros

* ECS decrypts the secrets straight from AWS to the ECS task environment. It **never** passes through your machine. IE: your laptop, a deploy server, or CodeBuild, etc.
* You do not have to grant permission for users to access and read the secret. Only the ecs-agent on the ECS Container Instance requires access.
* It's highly secure. Users can be given limited AWS credential permissions.

## Cons

* It requires setting up an IAM `executionRoleArn` permissions correctly. When you configure the `executionRoleArn` in the ECS Task Definition, this overrides the ECS provided one with reasonable defaults. You take on full responsibility. You now have to grant proper access to the ecs-agent. It's easy to forget things like ECR pull permission and can lead to a frustrating experience.
* Figuring out the secrets syntax takes some wrangling.
* ECS secrets can be difficult to debug. To see the error, you have to dig deep into the STOPPED task details. The errors are not well surfaced in the ECS console. The [ufo ps]({% link _reference/ufo-ps.md %}) tries to be helpful and surface errors when available.
* Secrets are pulled **lazily** at container start time by the ecs-agent. If the AWS secrets service is down, the container will not be able to spin up. Though AWS Secrets are unlikely to go down, it's a point of failure. Unlike secrets, simple environment variables and their values are embedded into your ECS Task Definitions.

You may think with the pros, using ECS Secrets support is what most will do. If you're in the high-security industry, the pros probably outweigh the cons. There is definitely overhead, and hassle in figuring it out can get in the way, though. Many are ok with using simple env vars. It's understandable, especially if they got a lot on their plate. It's a time trade-off, and companies usually have different priorities and requirements.
