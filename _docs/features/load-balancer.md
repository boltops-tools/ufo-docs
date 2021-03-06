---
title: Load Balancer Support
nav_text: Load Balancer
category: features
order: 2
---

UFO can automatically create a load balancer and associate it with an ECS service. Here are the default settings.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.elb.enabled = "auto"
  config.elb.port = 80
end
```

In `enabled = "auto"` mode, UFO will create a ELB  when `UFO_ROLE=web`. So by default, UFO will create an ELB. You can disable this behavior if you wish by setting `enabled = false`.

## ELB Types: Application and Network

UFO supports both Application and Network Load Balancers.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.elb.type = "network"
end
```

This will create a Network Load Balancer instead of an Application Load Balancer.

## ELB Static IP addresses for Network Load Balancers

You can also create a Network Load Balancer with pre-defined static IPs that you control.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.elb.subnet_mappings = [{
    AllocationId: "eipalloc-ac226fa4",
    SubnetId: "subnet-111",
  },{
    AllocationId: "eipalloc-b5206dbd",
    SubnetId: "subnet-222",
  }]
end
```

Using the static IP feature must be for a brand new deployment. Otherwise, you'll get this error:

    09:51:14PM UPDATE_FAILED AWS::ElasticLoadBalancingV2::LoadBalancer Elb Subnet removal is not supported for Network Load Balancers. You must specify all existing subnets along with any new ones (Service: AmazonElasticLoadBalancing; Status Code: 400; Error Code: ValidationError; Request ID: 034c7bc4-06b5-4de7-b72f-e08e7ce0a190; Proxy: null)

So if you later decide to use static IPs, one workaround is first to change the ELB to an Application Load Balancer, and then back again to a Network Load Balancer with the static IPs.

Otherwise, you must `ufo destroy` first and run `ufo ship` afterward.

{% include config/reference/header.md %}
{% include config/reference/elb.md %}
{% include config/reference/footer.md %}
