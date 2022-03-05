---
title: ufo central update
reference: true
---

## Usage

    ufo central update

## Description

update .ufo from central repo

The `UFO_CENTRAL_REPO` env var is required to use `ufo central update`.

## Example

    $ export UFO_CENTRAL_REPO=git@github.com:org/repo
    $ ufo central update
    Will create the .ufo folder.
    Are you sure? (y/N) y
    Updating .ufo with git@github.com:org/repo
    => git clone git@github.com:org/repo
    The .ufo folder has been updated
    $

## Subfolders

If the ufo folder is within a subfolder.

    export UFO_CENTRAL_FOLDER=subfolder

This will copy over the subfolder within the repo.


## Options

```
y, [--yes], [--no-yes]  # Bypass are you sure prompt
```

