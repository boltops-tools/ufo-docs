---
title: Security Groups
nav_text: Security Groups
categories: config
order: 4
---

## Considerations

UFO creates and manages two security groups. One for the ELB and one for the ECS tasks.  Here are some considerations for these security groups:

* Network load balancers do not support security groups. So an ELB security group is only created if the load balancer is an Application load balancer.
* The ECS security group for tasks always gets created, but is only used if network_mode is awsvpc. This is because in bridge network mode the EC2 container instance's Ethernet card and its security group is used. The EC2 containers group security group is outside the control of ufo. You'll need to configure the security group appropriately yourself. Usually, a security group inbound rule that allows port 49153-65535 to the VPC CDIR range is good. Also see: [Debugging Unhealthy Targets]({% link _docs/debug/unhealthy-targets.md %}).
* UFO will only assign the ECS security group when awsvpc node mode is used, and ufo has control of the security group.

## Disabling Managed Security Groups

You can disable the creation of managed security groups with: `vpc.security_groups.managed = false`. Example:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.vpc.security_groups.managed = false
end
```

You might want to do this if you're providing existing security groups.

Security Groups managed by UFO are, in a sense, transient. If you delete the CloudFormation stack created by UFO, and recreate the stack entirely. Any manual changes to the security groups are lost.

You can precreate security groups and configure UFO to use them. So then you won't lose any manual changes. If you're taking this approach, it's nice to have UFO not create any managed security groups at all to remove security group clutter.

## Using Existing Security Groups

You can tell UFO to use existing security groups like so

.ufo/config/env/dev/web.rb

```ruby
Ufo.configure do |config|
  config.vpc.security_groups.ecs = ["sg-111"]
  config.vpc.security_groups.elb = ["sg-111"]
end
```

The example above shows how to set security groups to override on an env-role scoped basis.

{% include config/reference/header.md %}
{% include config/reference/vpc.md %}
{% include config/reference/footer.md %}
