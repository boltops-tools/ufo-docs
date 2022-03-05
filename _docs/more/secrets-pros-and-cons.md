---
title: Secrets Pros and Cons
category: more
order: 8
---

## What are Secrets?

[ECS supports injecting secrets or sensitive data](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data.html) into the environment as variables.

We'll go over some of the pros and cons of using secrets.

## Pros

* ECS decrypts the secrets straight from AWS to the ECS task environment. It **never** passes through the machine calling `ufo ship` IE: your laptop, a deploy server, or CodeBuild, etc.
* The user does not even need IAM permission and access to the secret. Only the ecs-agent in the ECS

## Cons

* Permissions, permissions, permissions. It requires setting up an IAM `executionRoleArn` permissions. When you configure one, it overrides the ECS default provided role, and you take on the responsibility and have to remember things like giving access to pull from ECR.
* Figuring out the secrets syntax takes some wrangling.
* They can be difficult to debug. To see the error, you have to dig deep into the STOPPED task details. The errors are not well surfaced in the ECS console. The [ufo ps]({% link _reference/ufo-ps.md %}) can be very helpful. It tries to surface errors more quickly.

You may think with the Pros, using ECS secrets support is what most will do. The extra overhead and hassles can get in the way, though. Many are ok with using simple env vars, especially if they got a lot on their plate. It's a time trade-off, and companies usually have different priorities and requirements.
