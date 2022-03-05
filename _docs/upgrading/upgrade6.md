---
title: Upgrading to Version 6
short_title: Version 6
order: 1
categories: upgrading
---

In ufo v6, significant structural changes were made to improve the tool. The recommendation is to:

1. Build a new project and map the settings structure over.
2. Create a brand new ECS service.
3. Deploy blue-green style and point the new DNS to the new service.
4. Destroy the old service.

The default `UFO_ENV` was changed from `UFO_ENV=development` to `UFO_ENV=dev`, so this should create a new stack.

If you were already using the shorter name `UFO_ENV=dev`, then you'll need to use another name like `UFO_ENV=sbx` and then switch back over by repeating the blue-green process. Though it's a decent effort, it is a controlled and safe way to upgrade to ufo v6.
