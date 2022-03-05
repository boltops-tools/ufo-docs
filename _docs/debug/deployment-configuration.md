---
title: Debugging Deployment Configuration
nav_text: Deployment Configuration
category: debug
order: 5
---

If you're seeing an error about `minimumHealthyPercent` or `maximumPercent`:

    (service app1-web-dev-EcsService-8FMliG8m6M2p) was unable to stop or start tasks during a deployment because of the service deployment configuration. Update the minimumHealthyPercent or maximumPercent value and try again.

The error can happen if you have a [deployment configuration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ecs-service-deploymentconfiguration.html) that prevents containers from spinning up. For example, if you only have 1 container and you set a `ecs.maximum_percent = 150`. Then ECS is not permitted to deploy 2 containers or `200%`.

Try setting the `ecs.maximum_percent` and `ecs.minimum_healthy_percent` back to their defaults:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.ecs.maximum_percent = 200
  config.ecs.minimum_healthy_percent = 100
end
```
