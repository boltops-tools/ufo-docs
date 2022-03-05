---
title: Base Docker Image
category: features
order: 7
---

## Concept

Docker is fantastic and has given developers more power and control over their application's OS.  Sometimes building Docker images can be slow, though.  Docker layer caching technology helps speed up the process as Docker will only rebuild required layers. But sometimes, one minor dependency changes it results in having to rebuild many layers.

The `ufo docker base` commands allow you to build a Docker image from `Dockerfile.base` to be used as a cache and in the FROM instruction in the main `Dockerfile`.  This base Docker cache image technique substantially speeds up development if you have many project dependencies.  Without the Docker image cache that already has all of your packages, it will have to rebuild a lot.

## Pros and Cons

There are pros and cons to using this approach.  There are 2 hard problems in computer science:

1. Naming
2. Caching

The main con about this approach is if you forget to update the base Docker image, you will have cached artifacts that will not disappear unless you rebuild the base Docker image.  While some folks are completely against introducing this cache layer, some have love how much it speeds up their Docker workflow.  If you use this technique, you should set up some automation that rebuilds the base Docker image.

## Dynamic Dockerfile

For the case of building a new base Docker image, UFO supports dynamically creating a Dockerfile from a `Dockerfile.erb`.  If `Dockerfile.erb` exists, ufo uses it to generate a `Dockerfile` as a part of the build process.  This means that you should update the source `Dockerfile.erb` instead, as the `Dockerfile` will be overwritten.  If `Dockerfile.erb` does not exist, then ufo will use the `Dockerfile` instead.

## Example

Here's what the ``Dockerfile.erb`` looks like:

```Dockerfile
FROM <%= @base_image %>
# ...
CMD ["bin/web"]
```

The `@base_image` variable is grabbed from this file:

.ufo/state/data.yml

```yaml
dev:
  base_image: 112233445566.dkr.ecr.us-west-1.amazonaws.com/demo/base:base-2019-06-10T03-22-34-f91cdd350
prod:
  base_image: 778899001122.dkr.ecr.us-west-1.amazonaws.com/demo/base:base-2019-06-10T03-23-34-abccddxzy
```


The `Dockerfile.erb` has access to any defined in `state/data.yml` actually, so you can add your own if you wish.

The `base_image` key is automatically updated by [ufo docker base](http://ufoships.com/reference/ufo-docker-base/) when `Dockerfile.erb` exists.


When `UFO_ENV=prod`, it'll produce the following.

Dockerfile:

```Dockerfile
FROM 778899001122.dkr.ecr.us-west-1.amazonaws.com/demo/base:base-2019-06-10T03-23-34-abccddxzy
# ...
CMD ["bin/web"]
```

The above example demonstrates a good use case. You may want a different FROM statement in your Dockerfile on a per-environment basis if you're using different ECR repositories from different AWS accounts entirely. The FROM statement changes based on which AWS account you're using.

## Usage

The general steps are:

1. Create a `Dockerfile.erb` with `<%= @base_image %>`. Remove `Dockerfile` and add it to `.gitignore`.
2. Run: `ufo docker base` to generate `state/data.yml`
3. Run: `ufo ship` to build a Docker image using `Dockerfile.erb`.

Remember when using the `Dockerfile.erb`, the `Dockerfile` is overwritten. So you should update the `Dockerfile.erb`.

## Build Args

Why not use [build args](https://www.jeffgeerling.com/blog/2017/use-arg-dockerfile-dynamic-image-specification)?

Ufo uses a YAML file, so users will not have to remember to provide the build arg. It is also easy to update the `.ufo/state/data.yml` with the `ufo docker base` command.
