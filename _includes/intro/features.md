## UFO Features

* **Automation**: Builds the Docker image and deployments the ECS Service in one step.
* **CLI Customizations**: You can customize a plethora of [settings]({% link _docs/config.md %}) for the tool.
* **Exec**: Use [ufo exec]({% link _docs/intro/ufo-exec.md %}) to hop into a running container to debug.
* **Logs**: Use [ufo logs]({% link _reference/ufo-logs.md %}) to tail and follow the logs.
* **DRY**: Use the same Task Definition code with ERB templating to keep things DRY. You can write the Task Definition in either [YAML or JSON format]({% link _docs/features/task-definition-json.md %}).
* **Layering**: Use the same ECS Task Definition code to build multiple environments like dev and prod with [Layering]({% link _docs/layering.md %}).
* **Helpers**: Use [Built-In Helpers]({% link _docs/helpers/builtin.md %}) like `dockerfile_port`, `docker_image`, `family` to build the Task Definitions dynamically. You can also add your own [Custom Helpers]({% link _docs/helpers/custom.md %}) if needed.
* **IAM**: You can define and fully control the [IAM permissions]({% link _docs/intro/iam/task.md %}) used for the EC2 instance and Docker container.
* **Generators**: UFO ships with a [generators]({% link _reference/ufo-init.md %}) to help you get started with ECS quickly.
* **Hooks**: You can also run [boot hooks]({% link _docs/config/boot.md %}) early into the process and set things like `AWS_PROFILE`.
