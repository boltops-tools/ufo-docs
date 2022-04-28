---
title: Security Groups
nav_text: Security Groups
categories: config
order: 4
---

## Considerations

UFO creates and manages two security groups. One for the ELB and one for the ECS tasks.  Here are some considerations for these security groups:

* Network load balancers do not support security groups. So an ELB security group is only created if the load balancer is an Application load balancer.
* The ECS security group tasks only created if network_mode is awsvpc. Security groups can only be associated with ECS tasks when network mode is awsvpc. So there's no point in creating an ECS security for bridge mode.
* In bridge network mode the EC2 container instance's Ethernet card and its security group is used. The EC2 containers group security group is outside the control of ufo. You'll need to configure the security group appropriately yourself. Usually, a security group inbound rule that allows port 49153-65535 to the VPC CDIR range is good. Also see: [Debugging Unhealthy Targets]({% link _docs/debug/unhealthy-targets.md %}).
* UFO will only assign the ECS security group when awsvpc node mode is used, and ufo has control of the security group.

## Using Existing Security Groups

You can tell UFO to use existing security groups. Here's an example that shows how to set security groups to override on an env-role scoped basis.

.ufo/config/env/dev/web.rb

```ruby
Ufo.configure do |config|
  config.vpc.security_groups.ecs = ["sg-111"]
  config.vpc.security_groups.elb = ["sg-111"]
end
```

Security Groups managed by UFO are, in a sense, transient. If you delete the CloudFormation stack created by UFO, and recreate the stack entirely. Any manual changes to the security groups are lost. You can precreate security groups and configure UFO to use them. So then you won't lose any manual changes. More docs: [Security Groups]({% link _docs/more/security-groups.md %})

{% include config/reference/header.md %}
{% include config/reference/vpc.md %}
{% include config/reference/footer.md %}
