---
title: Built-In Helpers
nav_text: Built-In
order: 1
category: helpers
---

The `.ufo/resources/task_definitions/web.yml` has access to helper methods. These helper methods provide useful information to help you build different Task Definitions with the same code.

Helper  | Description
------------- | -------------
acm_cert(domain) | Get cert arn by domain. Returns first match.
docker_image | The Docker image that ufo builds. This is the full image name, including the added timestamp. IE: `org/repo:ufo-[timestamp]-[git-sha]`.
dockerfile\_port | Exposed port extracted from the Dockerfile. If the `EXPOSE` line in the Dockefile is changed, we sometimes forget to update the port in the Task Definition.  Reference the value via this helper prevents that mistake from happening.
ecr_repo(name) | Get repository uri arn by name. Returns first match.
env_file(path) | This method takes a `.env` file which contains a simple key-value list of environment variables and converts the list to the proper task definition JSON format.
env_vars(text) | This method takes a block of text that contains the env values in `key=value` format and converts that block of text to the proper task definition JSON format.
family | The family name of the task_definition. The default is `APP-ROLE-ENV`. IE: `demo-web-dev`.
role | The role. IE: web, worker, clock
secrets_file(path) | This method takes a `.secrets` file which contains a simple key-value list of environment variables and converts the list to the proper task definition JSON format. See: [Secrets Docs]({% link _docs/helpers/builtin/secrets.md %})
secrets_vars(text) | This method takes a block of text that contains the secrets values in `key=value` format and converts that block of text to the proper task definition JSON format. See: [Secrets Docs]({% link _docs/helpers/builtin/secrets.md %})
ssm(name) | Get ssm value.
stack_output(name) | Get stack output value. Note: Cannot use :APP in the expansion pattern. Can use :ENV though. `stack_output("vpc-:ENV.Vpc")` => `stack_output("vpc-dev.Vpc")`.

The helper methods generally defined in [task_definition/helpers](https://github.com/tongueroo/ufo/blob/master/lib/ufo/task_definition/helpers).

Note: The helpers can be used in config files as well as the Task Definition template `web.yml`. However, config files are loaded super early in the boot process. So when helpers are used in config files, they cannot use `:APP` as part of pattern expansion since `config.app` is assigned in the config files themselves.
