---
title: Layering Debugging Tips
nav_text: Debugging
category: layering
order: 88
---

UFO layering is a powerful ability. While some folks love layering, some dislike it. Think this is because layers are so powerful that they can get complex, especially when abused. It's up to the welder of the sword. This doc tries to help you debug layering, even when abused. 🤣

## Seeing Layers Clearly

Probably the best tip is to configure `config.layering.show` so you can see the layers.

{% include layering/config-layering-show.md %}

    $ ufo build --no-docker
    Config Layers
        .ufo/config.rb
        .ufo/config/web/base.rb
        .ufo/config/web/dev.rb
    Building Task Definition
        .ufo/vars/base.rb
        .ufo/vars/dev.rb


## All Layering

To see all the **possible** layers is to set `UFO_LAYERS_ALL=1`.

    export UFO_LAYERS_ALL=1

There are many layers, so would just choose a few that work for your team and stick to those.

    Config Layers
        .ufo/config.rb
        .ufo/config/base.rb
        .ufo/config/dev.rb
        .ufo/config/env.rb
        .ufo/config/env/base.rb
        .ufo/config/env/dev.rb
        .ufo/config/web.rb
        .ufo/config/web/base.rb
        .ufo/config/web/dev.rb
    Building Task Definition
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

As you can see, layering can be pretty powerful but also complex.

## Useful With

Showing layers to debug them is particularly useful when using:

* [App Layering]({% link _docs/layering/app.md %})
