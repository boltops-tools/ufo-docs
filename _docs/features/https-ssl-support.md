---
title: SSL Support
nav_text: HTTPS or SSL
category: features
order: 4
---

UFO supports creating a Load Balancer with SSL termination with ACM certs.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.elb.ssl.enabled = true
  config.elb.ssl.certificates = "arn:aws:acm:us-east-1:111111111111:certificate/11111111-2222-3333-4444-555555555555"
end
```

The `elb.ssl.certificates` option can be assigned a single item or an Array with multiple certs. For the certificate arn, you will need to create a certificate with AWS ACM. To do so, you can follow these instructions: [Request a Public Certificate
](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request-public.html)

The protocol will be HTTP or HTTPS for Application Load Balancers and TCP or TLS for Network Load Balancers. UFO will infer the correct value, so you don't have to configure the protocol manually.

## Helper Method

You can use the `acm_cert` [helper method]({% link _docs/helpers/builtin.md %}) to lookup the acm_cert with the domain.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.elb.ssl.enabled = true
  config.elb.ssl.certificates = acm_cert("example.com")
end
```

If you have multiple certs with the same domain, first cert in the API response is used. If you need more specificity, then you should use the full ARN.