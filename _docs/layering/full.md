---
title: Full Layering
category: layering
order: 3
---

The [Basic layering]({% link _docs/layering/basics.md %}) docs focuses on providing a gentle introduction to layering. This document covers the full power of layering.


## Full Variables Layers

Here's the full layering for variables:

    .ufo/vars.rb
    .ufo/vars/base.rb
    .ufo/vars/dev.rb
    .ufo/vars/web.rb
    .ufo/vars/web/base.rb
    .ufo/vars/web/dev.rb
    .ufo/vars/demo.rb
    .ufo/vars/demo/base.rb
    .ufo/vars/demo/dev.rb
    .ufo/vars/demo/web.rb
    .ufo/vars/demo/web/base.rb
    .ufo/vars/demo/web/dev.rb

## Full Config Layers

Here's the full layering for config:

    .ufo/config.rb
    .ufo/config/base.rb
    .ufo/config/dev.rb
    .ufo/config/env.rb
    .ufo/config/env/base.rb
    .ufo/config/env/dev.rb
    .ufo/config/web.rb
    .ufo/config/web/base.rb
    .ufo/config/web/dev.rb

## Recommendation

The layering is pretty powerful and can be too powerful. It can be confusing, like a matryoshka doll. So would pick a few that work with your team and stick to them.

{% include layering/config-layering-show.md %}
