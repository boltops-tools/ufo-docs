---
title: Using Existing Load Balancer Target Group
nav_text: Existing
category: load-balancer
order: 1
---

You can use an existing Load Balancer and its Target Group with the `elb.existing` settings. Example:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.elb.existing.target_group = "arn:aws:elasticloadbalancing:us-west-2:111111111111:targetgroup/elb-d-Targe-12NCI2V1X5TBS/ed56e555b7e8d0db"
  config.elb.existing.dns_name = "elb-dev-Elb-FOOAR4WRTPXKY-421647888.us-west-2.elb.amazonaws.com"
end
```

When using an existing Load Balancer, the other elb settings are ignored by UFO. This is because you're "bringing your own" Load Balancer, it's outside the control of UFO. You take on the responsibility of managing the ELB and its settings.

{% include config/reference/header.md %}
{% include config/reference/elb-existing.md %}
{% include config/reference/footer.md %}
