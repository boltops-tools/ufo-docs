---
title: Extra Environments
category: features
order: 6
---

Ufo has a concept of extra environments. It's as simple as setting the `UFO_EXTRA` env variable.

    ufo ship demo-web             # creates demo-web-dev stack
    UFO_EXTRA=2 ufo ship demo-web # creates demo-web-dev-2 stack

These environments have the same `UFO_ENV` value. Think about them as extra "instances" of an environment. This is useful if you want environments with the same environment settings. You can get extra dev, qa, etc environments for almost no effort.
