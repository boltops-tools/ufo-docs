---
title: Debugging CloudFormation Template
nav_text: CloudFormation Template
category: debug
order: 9
---

UFO ultmately builds a CloudFormation template with ECS resources and deploys it to AWS. It's useful to inspect this CloudFormation template for debugging. You can do it beforehand with these commands.

    ufo build
    cat .ufo/output/template.yml | jq

Once the Docker image has been built at least once on the machine you can forego the docker build step with `--no-docker`

    ufo build --no-docker
    cat .ufo/output/template.yml | jq

This is particularly useful to debug when you wish to inspect the CloudFormation template before. For example, if you're working with [env]({% link _docs/features/env_files/env.md %}) or [secrets]({% link _docs/features/env_files/secrets.md %}) files. It's useful to confirm that the Task definition has the secrets in the right format.
