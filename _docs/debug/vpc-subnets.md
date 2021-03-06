---
title: Debugging Invalid VPC Subnets
nav_text: Invalid VPC Subnets
category: debug
order: 9
---

If you see this error.

    11:32:02PM UPDATE_IN_PROGRESS AWS::ElasticLoadBalancingV2::LoadBalancer Elb
    11:32:03PM UPDATE_FAILED AWS::ElasticLoadBalancingV2::LoadBalancer Elb All subnets must belong to the same VPC: 'vpc-111111' (Service: AmazonElasticLoadBalancing; Status Code: 400; Error Code: InvalidConfigurationRequest; Request ID: ef73da4a-89a6-4469-90b9-4b4fbdc43f82; Proxy: null)
    11:32:04PM UPDATE_IN_PROGRESS AWS::EC2::SecurityGroupIngress EcsSecurityGroupRule Requested update requires the creation of a new physical resource; hence creating one.

It can happen if you already have deployed the ECS Service previously, have then changed the VPC settings, and tried deploying again. Example:

* The `config.vpc` subnets configurations have been changed in `.ufo/config.rb`.
* The `.ufo/resources/task_definitions/web.yml` networkMode has been changed from bridge to awsvpc mode or vice-versa. This also seems to cause the error.

## How to Resolve

Double-check the VPC settings. You might have to adjust the VPC settings so the ECS Service that UFO creates **matches** with the VPC that the ECS Cluster is running on.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  # Double check and adjust
  config.vpc.id = "vpc-111"
  config.vpc.subnets.ecs = ["subnet-111", "subnet-222"] # at least 2 required
  config.vpc.subnets.elb = ["subnet-111", "subnet-222"] # at least 2 required
end
```

Reconfigure it if that's the case, then then you may have to cancel and destroy the ECS service first and recreate it.

    ufo cancel
    ufo destroy
    ufo ship

## Why

This is because the underlying CloudFormation stack that UFO uses to update the ECS Service cannot make such a change. Remember, `ufo ship`, deploys the ECS Service with CloudFormation. It leverages CloudFormation to do the hard work of orchestration. CloudFormation uses other services like ECS, ELB, etc, to make the changes we ask for and can be limited by the underlying service.

Sometimes the underlying services may not support such a change. CloudFormation may try to replace the resource or update it in place. But sometimes it's not possible. CloudFormation tries to take the best path possible. In the case above, though somewhat cryptic, the error message seems to indicate that CloudFormation is trying to update and move the LoadBalancer to another VPC subnet, but it's having trouble doing so. Unsure exactly why CloudFormation does not create a new ELB in this case. There may be an ELB service related-reason for this.

In any case, the CloudFormation orchestration sequence is not able to make such a change.  These edge cases are usually rare but difficult to debug and can happen.

Also see: [Config VPC Docs]({% link _docs/config/vpc.md %})
