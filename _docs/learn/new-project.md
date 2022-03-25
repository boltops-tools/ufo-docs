---
title: New Project
---

If you already have a project with an existing Dockerfile, ufo will use that. If you do not, ufo can generate a starter Dockerfile that runs Nginx. For this tutorial, we’ll start with an empty folder.

    mkdir demo
    cd demo

## Docker Repo

You'll need a Docker repo to push the Docker image to. For this guide, we'll use ECR.

    aws ecr create-repository --repository-name demo
    REPO=$(aws ecr describe-repositories --repository-name demo | jq -r '.repositories[].repositoryUri')

You created the repo and saved the `repositoryUri` to the `REPO` shell variable.  The REPO will look something like this:

    $ echo $REPO
    111111111111.dkr.ecr.us-west-2.amazonaws.com/demo

## Initialize Structure

Now, to initialize a project and set it up for UFO. This is similar to a `git init`.

    ufo init --app demo --repo $REPO

The output should look something like this:

    $ ufo init --app demo --repo $REPO
    Generating .ufo structure
          create  .ufo/config.rb
          create  .ufo/config/web/base.rb
          create  .ufo/config/web/dev.rb
          create  .ufo/config/web/prod.rb
          create  .ufo/resources/iam_roles/execution_role.rb
          create  .ufo/resources/iam_roles/task_role.rb
          create  .ufo/resources/task_definitions/web.yml
          create  .ufo/vars/base.rb
          create  .ufo/vars/dev.rb
          create  .ufo/vars/prod.rb
          create  .gitignore
          create  .dockerignore
          create  Dockerfile

Let's explore some of the generated files.

## Review Dockerfile

The `Dockerfile` is a simple starter example that runs Nginx.

Dockerfile:

    FROM nginx
    EXPOSE 80
    CMD ["nginx", "-g", "daemon off;"]

Note: If your project already has a Dockerfile, ufo will use that instead of generating a new one.

## Review Config

The `config.rb` is where you can configure UFO settings.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.logger.level = "info" # IE: info or debug
  config.app = "demo" # env var UFO_APP takes higher precedence if set
  config.docker.repo = "111111111111.dkr.ecr.us-west-2.amazonaws.com/demo"
  config.ecs.cluster = ":ENV" # pattern is replaced with UFO_ENV. Default is UFO_ENV=dev
end
```

This is where the `--app` and `--repo` options from `ufo init` got saved.

The `ecs.cluster` option tells UFO what ECS cluster to use. Its pattern gets expanded. IE: For `UFO_ENV=dev`, the `dev` ECS cluster is used.

The other options are covered in the [Config Docs]({% link _docs/config.md %}).

Next, we'll review the main `.ufo` files you'll work with.
