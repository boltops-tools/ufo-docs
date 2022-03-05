---
title: Boot Hooks
nav_text: Boot Hooks
category: config
order: 6
---

If you need to hook into the Ufo boot process super early on, Ufo boot hooks are designed for that.

* They run very early in the Ufo boot process.
* They are useful for setting shared global values like env vars.
* Boot hooks are ruby files that get required. It's nice and simple. There's no interface to learn.

## Hooks

Ufo will search 2 files in the `config` folder. If the files exist, they will be run in this order.

1. **config/boot.rb**: Always runs.
2. **config/boot/UFO_ENV.rb**: Runs based on the env. IE: `UFO_ENV=dev` => `config/boot/dev.rb`

Both files are required and run if they both exist. Since the `UFO_ENV` one runs second, it can be used to override previously set values.

## Example: Default UFO_ENV

If you prefer a different default than `UFO_ENV=dev`.

config/boot.rb

```ruby
ENV['UFO_ENV'] ||= 'prod'
```

This changes the default for everyone using the project but still allows them to control the default by adding `export UFO_ENV=dev` to their `~/.bash_profile`.

## Example: Auto-Switch AWS_PROFILE

One useful example is switching `AWS_PROFILE` based on the `UFO_ENV`. Example:

config/boot/dev.rb

```ruby
ENV['AWS_PROFILE'] = 'dev'
```

This example is for `AWS_PROFILE`, but you can do similar switch logic with other env vars, etc.

## Boot Source

Please refer to the boot source code for more details: [ufo/booter.rb](https://github.com/boltops-tools/ufo/blob/master/lib/ufo/booter.rb)
