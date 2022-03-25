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

In this case `UFO_APP=demo` and `UFO_ENV=dev`. The app can also configured with `config.app` in ``.ufo/config.rb`.

## Overrride Convention

The conventional naming scheme can be customized. Here’s a config with the default naming pattern. You can change it if you want.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.secrets.ssm_pattern = :APP-:ENV-::NAME
end
```

You can also change the default secrets provider to secrets manager and its pattern.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.secrets.manager_pattern = :APP-:ENV-::NAME
  config.secrets.provider = secretsmanager
end
```

{% include features/env_files/layering.md %}

Related Docs: Also See [Helper Secrets]({% link _docs/helpers/builtin/secrets.md %}).
