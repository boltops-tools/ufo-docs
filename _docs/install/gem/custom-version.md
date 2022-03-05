---
title: How to Use Custom Version of Ufo
nav_text: Custom Version
category: gem
order: 2
---

If you want or need to run a forked version of ufo, here's how:

## Change Version Number

First, clone down the source and change the version number so you can identify that it's a custom version.  Clone down the project:

    $ git clone https://github.com/boltops-tools/ufo

Update [lib/ufo/version.rb](https://github.com/boltops-tools/ufo/blob/master/lib/ufo/version.rb) so you'll be able to tell that it's a custom build.  For example:

```ruby
module Ufo
  VERSION = "6.0.0.custom"
end
```

## Gem Package: Build and Install

Build the gem package:

    $ bundle
    $ rake build
    pkg/ufo-6.0.0.custom.gem

The produced `.gem` file can used to install the gem. You can install it locally:

    $ gem install pkg/ufo-6.0.0.custom.gem
    $ ufo -v
    6.0.0.custom

Or on any machine with Ruby installed. You can copy it to the machine, ssh into it, and install the custom gem:

    $ scp pkg/ufo-6.0.0.custom.gem user@server.com:
    $ ssh user@server.com
    $ gem install ufo-6.0.0.custom.gem
    $ ufo -v
    6.0.0.custom

## Contributing

Learning how to run a forked version of ufo allows you to change the tool. Consider improving ufo by submitting a Pull Request. See: [Contributing]({% link _docs/contributing.md %}).
