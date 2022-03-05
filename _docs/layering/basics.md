---
title: Layering Basics
nav_text: Basics
category: layering
order: 1
---

## Variables

Variables support layering. Here's a simple starter layers structure:

    .ufo/vars
    ├── base.rb
    ├── dev.rb
    └── prod.rb

Your task definition like `.ufo/resources/task_definitions/web.json` is processed by ERB templating and can make of variables.

With layering, you can set and use common `base.rb` variables for your task definitions. And then override env specific values like so:

    export UFO_ENV=dev
    ufo ship # uses .ufo/vars/base.rb and .ufo/vars/dev.rb

For prod:

    export UFO_ENV=prod
    ufo ship # uses .ufo/vars/base.rb and .ufo/vars/prod.rb

* `base.rb` is always evaluated.
* `dev.rb` or `prod.rb` is evaluated based on `UFO_ENV`.

## More

More step-by-step docs are provided in the following documentation pages and details layering further.
