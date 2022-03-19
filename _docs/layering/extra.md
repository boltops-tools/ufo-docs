---
title: Extra Layering
category: layering
order: 5
---

If you're setting the `UFO_EXTRA` env var to leverage the Ufo.extra feature, it also activate an extra layer.

## Example

    .ufo/vars
    ├── base.rb
    ├── dev.rb
    └── dev-2.rb <= extra layer

{% include layering/config-layering-show.md %}

Here are some of that layers shown.

    $ ufo build --no-docker
    Config Layers
        .ufo/config/base.rb
        .ufo/config/dev.rb
        .ufo/config/dev-2.rb
    Building Task Definition
        .ufo/vars.rb
        .ufo/vars/base.rb
        .ufo/vars/dev.rb
        .ufo/vars/dev-2.rb

