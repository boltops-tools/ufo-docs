---
title: DNS Route53 Support
nav_text: DNS Route53
category: features
order: 3
---

UFO can create a "pretty" route53 record and value to the ELB DNS name. Example:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.dns.domain = "domain.com" # only recommended option to set
end
```

When the `dns.domain` is configured, UFO will create a conventional Route53 record and point it to the ELB DNS name.

    demo-web-dev.domain.com -> demo-we-Elb-7DSB8ODFY8QP-1678773019.us-west-2.elb.amazonaws.com

This is the recommended way to configure managed DNS by UFO. You can then manually CNAME your domain to the conventional DNS record. Example:

    www.domain.com -> demo-web-dev.domain.com -> demo-we-Elb-7DSB8ODFY8QP-1678773019.us-west-2.elb.amazonaws.com

This allows you complete control of the user-facing DNS record.

**IMPORTANT**: The route53 host zone must already exist. You can create route53 hosted zone with the AWS CLI like so:

    aws route53 create-hosted-zone --name mydomain.com --caller-reference $(date +%s)
    aws route53 list-hosted-zones

{% include config/reference/header.md %}
{% include config/reference/dns.md %}
{% include config/reference/footer.md %}
