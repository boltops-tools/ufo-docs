---
title: Debugging Failed to Register Targets
nav_text: Failed Register Targets
category: debug
order: 8
---

If you're seeing a Failed to Register Targets error like:

    (service demo-web-dev-EcsService-gOCz3YOQTvV9) failed to register targets in (target-group arn:aws:elasticloadbalancing:us-west-2:111111111111:targetgroup/demo-Targe-H7QJOLZK1CDM/3cc9518a30c0f229) with (error The following targets are not in the target group VPC 'vpc-11111111': 'i-0381a63c79abcf413')

This means your ECS Cluster is in another VPC than the subnets you're providing to the ECS Service.  Try adjusting the config vpc settings so that they match.  Here are some examples:

## Default VPC

If the ECS Cluster is running in the default VPC, you don't have to configure anything. The default VPC is used when you do not configure `config.vpc` settings.

## Custom VPC

If the ECS Cluster is running in a custom VPC that was built with CloudFormation. You must configure the `config.vpc` settings

.ufo/config.rb


```ruby
Ufo.configure do |config|
  config.vpc.id = stack_output("vpc-:ENV.Vpc")
  config.vpc.subnets.ecs = stack_output("vpc-:ENV.PrivateSubnets").split(',')
  config.vpc.subnets.elb = stack_output("vpc-:ENV.PublicSubnets").split(',')
end
```

Here the CloudFormation stack is named `vpc-dev` with outputs that contain the vpc id and subnet ids.

{% include config/reference/header.md %}
{% include config/reference/vpc.md %}
{% include config/reference/footer.md %}
