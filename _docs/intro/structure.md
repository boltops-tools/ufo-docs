---
title: Project Structure
nav_text: Structure
category: intro
order: 2
---

UFO creates a `.ufo` folder within your project, which contains the required files used by UFO to build and deploy docker images to ECS.

## The Structure

Here's the basic `.ufo` structure.

    .ufo
    ├── config.rb
    ├── resources
    │   ├── iam_roles
    │   │   ├── execution_role.rb
    │   │   └── task_role.rb
    │   └── task_definitions
    │       └── web.yml
    └── vars
        └── web
            │── base.rb
            │── dev.rb
            └── prod.rb

You'll mostly work with `resources/task_definitions/web.yml` and `vars/web`.

Note: You can use either `web.yml` or `web.json`. Both formats are supported.

Hopefully, that gives you a basic idea of the structure.

## Structure Reference

The table below covers more of the structure and the purpose of each folder and file.

File / Directory  | Description
------------- | -------------
config.rb  | UFO's general settings file, where you adjust the default [config]({% link _docs/config.md %}).
config/env/dev.rb  | Env specific config settings.
output/  | The folder where the generated task definitions are written to.
resources/iam_roles/  | Where ufo managed iam roles associated with the task definition can be defined. For more details see: [IAM Roles]({% link _docs/intro/iam/task.md %}).
resources/task_definitions  | This is where you define the task definitions and specify the variables used by the ERB templates.
vars  | This is where you can define shared variables available to your task definition templates. More info at [Variables]({% link _docs/layering/variables.md %}).

