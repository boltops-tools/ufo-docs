---
title: VPC Settings
nav_text: VPC
categories: config
order: 3
---

You can configure network settings with `config.vpc`. Example:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.vpc.id = "vpc-111"
  config.vpc.subnets.ecs = ["subnet-111", "subnet-222"] # at least 2 required
  config.vpc.subnets.elb = ["subnet-111", "subnet-222"] # at least 2 required

  # Optional existing security group ids to add in addition to the ones created by ufo.
  config.vpc.security_groups.ecs = ["sg-111"]
  config.vpc.security_groups.elb = ["sg-111"]
end
```

## Stack Output Helper

If you have built a VPC with CloudFormation and it has outputs with the network information, you can use the `stack_output` helper method to grab the information. Example:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.vpc.id = stack_output("vpc-:ENV.Vpc")
  config.vpc.subnets.ecs = stack_output("vpc-:ENV.PrivateSubnets").split(',')
  config.vpc.subnets.elb = stack_output("vpc-:ENV.PublicSubnets").split(',')
end
```

{% include config/reference/header.md %}
{% include config/reference/vpc.md %}
{% include config/reference/footer.md %}
