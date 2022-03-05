---
title: Central Deployer Pattern
nav_text: Central Deployer
categories: patterns
order: 1
---

UFO can be use as either an app-centric or ops-centric tool.

* **app-centric**: Each app repo has it's own `.ufo` settings files. This is useful if your applications are very differently setup.
* **ops-centric**: Each app repo has pretty much the same `.ufo` settings files. This is useful if your applications are very similarly set up.

## Setup

With an ops-centric approach, you use the same `.ufo` settings files for the app repos you want to use. For example:

    https://github.com/org/app1
    https://github.com/org/app2

You then also have a

    https://github.com/org/ufo-central

You can copy the `ufo-central` repo into the app repo folder with the [ufo central update]({% link _reference/ufo-central-update.md %}) command like so:

    cd app1
    ufo central update

You'll end up with something like this:

    app1/.ufo
    app2/.ufo

Then to deploy different app level settings.

For app1:

    cd app1
    export UFO_APP=app1
    ufo ship

And for app2:

    cd app1
    export UFO_APP=app2
    ufo ship

Also check out: [Layering]({% link _docs/layering.md %}) and [IAM Role Docs]({% link _docs/intro/iam/task.md %}).

The central deployer approach is removes duplication of the ufo config files between projects. Leveraging app-level overrides gives provides a great degree of control.

If the settings start to diverge too much, then it probably makes sense to use separate `.ufo` files in that app specific repo.
