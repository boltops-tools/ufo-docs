---
title: Layering
category: features-env-files
order: 5
---

Env files support layering also.

## Basic Example

    .ufo/env_files/
    ├── base.env
    ├── dev.env
    └── prod.env

## App and Role Layering

Those layers are also applied based on what the app and role are set to. Here's an example with `UFO_APP=demo` and `UFO_ROLE=web`.

    .ufo/env_files/demo/base.env
    .ufo/env_files/demo/dev.env
    .ufo/env_files/demo/web.env
    .ufo/env_files/demo/web/base.env
    .ufo/env_files/demo/web/dev.env

The layering applies for secret files also.

    .ufo/env_files/demo/base.secrets
    .ufo/env_files/demo/dev.secrets
    .ufo/env_files/demo/web.secrets
    .ufo/env_files/demo/web/base.secrets
    .ufo/env_files/demo/web/dev.secrets
