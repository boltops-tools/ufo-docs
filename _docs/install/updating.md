---
title: Updating UFO
---

Typically, ufo is used as a standalone tool. For example, there's no project Gemfile and ufo is not a project library dependency.

## System Install

Update using the installer you used to install ufo onto the system or server. For example:

    gem install ufo

## Project Dependency

Though not as typical, you might have ufo as one of your project dependencies. If so, you'll also need to update to update the pinned ufo version in your project's `Gemfile`. It looks something like this.

Gemfile

```ruby
gem "ufo", '~> 6.0.0'
```

So to update ufo to the next version, remove the version specifier `'~> 6.0.0'` entirely or update it to the version you wish.  Then run:

    bundle update ufo

This generates a new `Gemfile.lock` that will pin a new ufo version down. To confirm that the desired version of ufo is being used:

    bundle info ufo

Note: When you are using ufo in this manner, you should use `bundle exec`, IE:

    bundle exec ufo

This ensures a pinned ufo is used. If typing bundle exec gets old, recommend writing a bash shim that adds `bundle exec` when needed.

## Updating Everything

If you want to update all dependencies. You can do that by running:

    bundle update

This updates all gems defined in the `Gemfile` and still try to keep implied dependencies generally pinned.

Often, you want to update all gems, including implied dependencies, completely. Blow away the `Gemfile.lock` and run `bundle update`. Example:

    rm -f Gemfile.lock
    bundle update

This generates a brand new `Gemfile.lock`. To see what versions are used:

    bundle list
