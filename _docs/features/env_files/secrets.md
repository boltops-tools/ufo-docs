---
title: Secrets
nav_text: Secrets
category: features-env-files
order: 2
---

Secrets files contain only references, not actual values, to sensitive data. This also makes them safe to commit to version control.

## Example

.ufo/env_files/dev.secrets

    DATABASE_URL=ssm:demo/dev/DATABASE_URL

.ufo/env_files/prod.secrets

    DATABASE_URL=ssm:demo/prod/DATABASE_URL

Secrets are made available as env vars, just like env files.

A key difference is that secrets are loaded into the environment lazily at container run-time. They are **never** passed through the local or deploy machine.

## Conventional Naming

Secrets files support a conventional naming scheme. So

.ufo/env_files/dev.secrets

    DATABASE_URL

Is the same as:

.ufo/env_files/dev.secrets

    DATABASE_URL=ssm:demo/dev/DATABASE_URL

In this case `UFO_APP=demo` and `UFO_ENV=dev`. The app can also configured with `config.app` in `.ufo/config.rb`.

## Secret Names

ECS secrets support is able to resolve the secret name with the added chars by Secrets Manager like so:

.ufo/env_files/dev.secrets

    PASS=secretsmanager:demo/dev/PASS-aBcDef

ECS secrets support also able to resolve the secret name without th added chars by Secrets Manager like so:

.ufo/env_files/dev.secrets

    PASS=secretsmanager:demo/dev/PASS

Hence the naming convention works just the same for secretsmanager

.ufo/env_files/dev.secrets

    PASS # also works. expands to PASS=secretsmanager:demo/dev/PASS

## Overrride Conventions

The default naming convention can be customized. Here’s a config with the default naming pattern. You can change it if you want.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.secrets.ssm_pattern = ":APP/:ENV/:NAME"
end
```

You can also change the default secrets provider to secrets manager and its pattern.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.secrets.manager_pattern = ":APP/:ENV/:NAME"
  config.secrets.provider = "secretsmanager"
end
```

## One-Off Override

You can also override the secrets provider by specifying the provider name in the .secrets file. Example:

.ufo/env_files/dev.secrets

    DATABASE_URL=secretsmangaer # one-off override

This expands to

    DATABASE_URL=secretsmangaer:demo/dev/DATABASE_URL

{% include features/env_files/layering.md %}

Related Docs: Also See [Helper Secrets]({% link _docs/helpers/builtin/secrets.md %}).
