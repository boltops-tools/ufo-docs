---
title: App, Role, and Env
category: intro
order: 5
---

There are 3 env variables that ufo uses that are good to know.

Name | Default | Description
---|---|---
UFO_ENV | dev | The environment like dev or prod.
UFO_APP | nil | This is nil by default because it's usually configured with `config.app` in [.ufo/config.rb]({% link _docs/config.md %}), which makes UFO_APP optional.
UFO_ROLE | web | The role like web, worker, or clock.

They are all env vars optional. Next, we'll cover the variables in an order that helps learning.

## UFO_ENV

This is pretty straightforward. You can use this to create and deploy different environments. Example:

    UFO_ENV=dev  ufo ship
    UFO_ENV=prod ufo ship

## UFO_ROLE

The role is a concept inspired from heroku. Apps often have a notion of a web, worker, and clock role. The default role is `web`. For example, if you configured an `app` name of `demo` like so:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.app = "demo"
end
```

Since the default is `UFO_ROLE=web`, running

    ufo ship

Creates a `demo-web-dev` stack. The role is in the middle, IE: `demo-:ROLE-dev`. You can switch roles like so:

    UFO_ROLE=worker ufo ship
    UFO_ROLE=clock  ufo ship

This will create `demo-worker-dev` and `demo-clock-dev` stacks, respectively.

## UFO_APP

The `UFO_APP` is the last one to cover and is interesting because you usually configure the app in `.ufo/config.rb` with `config.app = "demo"`. The `UFO_APP` variable allows you to override the app and activates app-level layering. This allows you to use the [Central Deploy Pattern]({% link _docs/patterns/central-deployer.md %}). When using `UFO_APP`, there's no need to configure `config.app`.

