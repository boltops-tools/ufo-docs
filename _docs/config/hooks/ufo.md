---
title: UFO Hooks
nav_text: UFO
categories: hooks
order: 3
---

{% include config/hooks/generator.md type="ufo" %}

You can use hooks to run scripts at specific steps of the `ufo` lifecycle.

## UFO Hooks

Hook | Description
---|---
build | When ufo builds the Task Definition and CloudFormation template. Build is automatically run as part of `ufo ship`. You can also call `ufo build`.
destroy | When deletes the ECS resources. IE: `ufo destroy`
ship | When ufo deploys to ECS. IE: `ufo ship`.

## Examples

These lifecycle points at the `ufo` command level. Here's are examples to help explain.

{% assign docs = site.docs | where: "categories","hooks-ufo" %}
{% for doc in docs -%}
* [{{ doc.title }}]({{ doc.url }})
{% endfor %}

{% include config/hooks/options.md command="ufo" %}
