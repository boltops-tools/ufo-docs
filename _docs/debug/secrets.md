---
title: SecretsManager Json Key
nav_text: SecretsManager
category: debug
order: 9
---

The ECS Secrets docs state that you can specify the json key when referencing the secret like so:

    arn:aws:secretsmanager:region:aws_account_id:secret:secret-name:json-key:version-stage:version-id

* [ECS Docs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data-secrets.html)

When testing it get this error though.

> Task Stopped Reason: Trying to retrieve secret with value arn:aws:secretsmanager:us-west-2:111111111111:secret:mysecret:k1 resulted in error: an invalid ARN format for the AWS Secrets Manager secret was specified. Specify a valid ARN and try again..

If anyone has successfully been able to use `json-key` please let me know.
