---
title: Base Docker Image
category: features
order: 7
---

## Concept

Docker is fantastic and has given developers more power and control over their application's OS.  Sometimes building Docker images can be slow, though.  Docker layer caching technology helps speed up the process by only rebuilding required layers. But sometimes, one minor change results in having to rebuild many layers.

The slowness is exacerbated when using CI Build systems like [AWS CodeBuild](https://aws.amazon.com/codebuild/), [GitHub Actions](https://github.com/features/actions), [CircleCI](https://circleci.com/), etc to build Docker images.  This is because managed build services usually create a fresh build machine to ensure consistency.  It's difficult for these services to leverage Docker layer caching effectively.

For example, even though AWS CodeBuild has a [LOCAL_DOCKER_LAYER_CACHE](https://docs.aws.amazon.com/codebuild/latest/userguide/build-caching.html) option, the cache is a best effort and won't be used if your builds don't run often enough. IE: Within 15 mins or so. See: [GitHub Issue 194 Local docker layer cache lifespan is too short](https://github.com/aws/aws-codebuild-docker-images/issues/194). Similarly, CircleCI
has a [setup_remote_docker](https://circleci.com/docs/2.0/docker-layer-caching/) feature that tries to speed up the build process. It also has its own limitations with cache misses.

## Base Image Approach

UFO supports building a Docker base image to help speed up the build process. The `ufo docker base` commands builds a Docker image from `Dockerfile.base`. It can then be used as a guaranteed cache in the FROM instruction for the main `Dockerfile`.  This base Docker cache image technique can substantially speeds up the build process.

## Pros and Cons

There are pros and cons to using this approach.  As the adage goes, there are 2 hard problems in computer science:

1. Naming
2. Caching

The main con about this approach is if you forget to update the base Docker image, you will have cached artifacts that will not disappear unless you rebuild the base Docker image.  While some folks are not keen on this cache layer, some have loved how much it speeds up their Docker workflow.  If you use this technique, you should probably set up automation that rebuilds the base Docker image on a scheduled basis.

## Dynamic Dockerfile

For the case of building a new base Docker image, UFO supports dynamically creating a Dockerfile from a `Dockerfile.erb`.

**Why?**

You may want a different `FROM` statement in your Dockerfile on a per-environment basis. For example, you've segregated your environments like dev and prod on separate AWS accounts for security. The Docker `FROM` statements could have different ECR repos from different AWS accounts.

You could also allow different AWS accounts like prod to read ECR images from the dev AWS accounts. While some are okay with this, many prefer strict security boundary permissions between the AWS accounts.

## How It Works

If `Dockerfile.erb` exists, UFO uses it to generate a `Dockerfile` as a part of the build process. Here's what the `FROM` statement in a `Dockerfile.erb` looks like:

Dockerfile.erb

```Dockerfile
FROM <%= @base_image %>
```

The `@base_image` variable is read from state stored by UFO. This state can be stored on s3. See: [Config State Docs]({% link _docs/config/state.md %})

The [ufo docker base](http://ufoships.com/reference/ufo-docker-base/) command automates the process, including updating the state data and updating the existing Dockerfile FROM statement.

When `UFO_ENV=dev`, it'll produce the following.

Dockerfile

```Dockerfile
FROM 1111111111111.dkr.ecr.us-west-1.amazonaws.com/demo/base:base-2022-15-10T03-23-34-foobarabc
```

When `UFO_ENV=prod`, it'll produce the following.

Dockerfile

```Dockerfile
FROM 222222222222.dkr.ecr.us-west-1.amazonaws.com/demo/base:base-2022-16-10T03-23-34-foobarxzy
```

## Usage

The general steps are:

1. Create both `Dockerfile.base` and `Dockerfile.erb` with `<%= @base_image %>`. Remove `Dockerfile` and add it to `.gitignore`.
2. Run: `ufo docker base` to update the state with the built Docker base image. Probably should be automated on a scheduled basis.
3. Run: `ufo ship` to build a Docker image using `Dockerfile.erb`.

**Important**: Remember when using `Dockerfile.erb`, you should update the source `Dockerfile.erb` instead of `Dockerfile`. The `Dockerfile` is auto-generated. You should `.gitignore` the `Dockerfile`.

## Build Args

Why not use [build args](https://www.jeffgeerling.com/blog/2017/use-arg-dockerfile-dynamic-image-specification)?

UFO automates this process, so users will not have to remember to provide the build arg. Found that it is too easy to forget to specify the build args, and there are still a lot of manual steps aside from the use of build args.
