---
title: Secrets
category: builtin-helpers
---

## What are Secrets?

[ECS supports injecting secrets or sensitive data](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data.html) into the environment as variables.  ECS decrypts the secrets straight from AWS to the ECS task environment. It **never** passes through your machine. IE: your laptop, a deploy server, or CodeBuild, etc.

Related: [ECS Secrets Pros and Cons]({% link _docs/more/secrets-pros-and-cons.md %})

## ECS Secrets Support

ECS supports 2 storage backends for secrets:

1. [Secrets Manager](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data-secrets.html#secrets-envvar)
2. [Systems Manager Parameter Store](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data-parameters.html#secrets-envvar-parameters)

Here are both of the formats:

Secrets manager format:

```json
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:secret_name-AbCdEf"
    }]
  }]
}
```

Parameter store format:

```json
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:ssm:region:aws_account_id:parameter/parameter_name"
    }]
  }]
}
```

## UFO Support

UFO supports both forms of secrets. You create a `.secrets` file and can reference it in

.ufo/resources/task_definitions/web.yml

```yaml
containerDefinitions:
  secrets: <%= secrets_file(".secrets").to_json %>
```

## Conventions over Configuration

The simplest way to use secrets is to provide a list of secret names in the `.secrets` file. For example:

.secrets

    DB_USER
    DB_PASS

UFO automatically expands using the UFO app name and UFO_ENV values. It works like this:

    DB_USER=arn:aws:ssm:region:aws_account_id:parameter/:APP/:ENV/DB_USER
    DB_PASS=arn:aws:ssm:region:aws_account_id:parameter/:APP/:ENV/DB_PASS

Ultimately, becoming something like this.

    DB_USER=arn:aws:ssm:us-west-2:11111111111:parameter/demo/dev/DB_USER
    DB_PASS=arn:aws:ssm:us-west-2:11111111111:parameter/demo/dev/DB_PASS

Sadly, Secrets Manager names have a unique id at the end, so this convention doesn't work with Secrets Manager.

## Override the Conventions

You can also set the expansion pattern to use and override the conventions.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.secrets.pattern.ssm = ":APP/:ENV/:SECRET_NAME" # => demo/dev/DB_PASS
end
```

The `config.secrets.pattern.ssm` option can be assigned a callable option, similiar to how `config.names.stack` can be assigned a callable option for even more custom control. See: [Config Names]({% link _docs/config/names.md %}). For secrets, the argument passed to the `.call(arg)` method is an instance of [Helper/Vars](https://github.com/boltops-tools/ufo/blob/master/lib/ufo/task_definition/helpers/vars.rb), though.

## Direct Control

The `.secrets` file is like an env-file that will understand a secrets-smart format.  Example:

    NAME1=SSM:my/parameter_name
    NAME2=SECRETSMANAGER:my/secret_name-AbCdEf

The `SSM:` and `SECRETSMANAGER:` prefix will be expanded to the full ARN. You can also specify the full ARN.

    NAME1=arn:aws:ssm:region:aws_account_id:parameter/my/parameter_name
    NAME2=arn:aws:secretsmanager:region:aws_account_id:secret:my/secret_name-AbCdEf

In turn, this generates:

```json
{
  "containerDefinitions": [{
    "secrets": [
      {
        "name": "NAME1",
        "valueFrom": "arn:aws:ssm:us-west-2:111111111111:parameter/demo/dev/foo"
      },
      {
        "name": "NAME2",
        "valueFrom": "arn:aws:secretsmanager:us-west-2:111111111111:secret:demo/dev/my-secret-test-qRoJel"
      }
    ]
  }]
}
```

## SSM Parameter Names with Leading Slash

If your SSM parameter has a leading slash, do **not** include it. Example:

    aws ssm get-parameter --name /demo/dev/foo

So use:

    FOO=SSM:demo/dev/foo

The extra slash confuses ECS. If you accidentally include it, UFO will remove it to avoid ECS provisioning errors. For secretsmanager names, you **can** include a leading slash.

## Substitution

UFO also does a simple substitution on the value. For example, the `:ENV` is replaced with the actual value of `UFO_ENV=dev`. Example:

    NAME1=SSM:demo/:ENV/parameter_name
    NAME2=SECRETSMANAGER:demo/:ENV/secret_name-AbCdEf

Expands to:

    NAME1=arn:aws:ssm:region:aws_account_id:parameter/demo/dev/parameter_name
    NAME2=arn:aws:secretsmanager:region:aws_account_id:secret:demo/dev/secret_name-AbCdEf

## IAM Permission

If you're using secrets, you'll need to provide an IAM execution role, so the EC2 instance has permission to read the secrets. Here's a starter example:

.ufo/iam_roles/execution_role.rb

```ruby
iam_policy("SsmParameterStore",
  Action: [
    "ssm:GetParameters",
  ],
  Effect: "Allow",
  Resource: "*"
)
iam_policy("SecretsManager",
  Action: [
    "secretsmanager:GetSecretValue",
  ],
  Effect: "Allow",
  Resource: "*"
)

managed_iam_policy("service-role/AmazonECSTaskExecutionRolePolicy")
```

More info [ECS IAM Roles]({% link _docs/intro/iam/task.md %})

## Debugging Tip

Be sure that the secrets exist. If they do not, you will see an error like this in the ecs-agent.log:

/var/log/ecs/ecs-agent.log

    level=info time=2020-06-26T00:59:46Z msg="Managed task [arn:aws:ecs:us-west-2:111111111111:task/dev/91828be6a02b48f982cd9122db5e39b2]: error transitioning resource [ssmsecret] to [CREATED]: fetching secret data from SSM Parameter Store in us-west-2: invalid parameters: /my-parameter-name" module=task_manager.go

Sometimes there is even no error message in the ecs-agent.log. As a debugging step, try removing all secrets and seeing if the container will start up.

## Disable Warning

UFO warns you if `.env` or `.secrets` files are missing. You can disable the warning message with:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.secrets.warning = false
end
```
