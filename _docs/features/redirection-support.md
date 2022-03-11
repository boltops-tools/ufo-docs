---
title: Redirection
category: features
order: 5
---

You can configure redirection by configuring the ELB Listener. Here's an example that redirects http to https with a 302 status code:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.elb.redirect.enabled = true
  # defaults
  # config.elb.redirect.code = 302  # IE: 302 or 301
  # config.elb.redirect.port = 443
  # config.elb.redirect.protocol = HTTPS

  # required to use the redirect above
  config.elb.ssl.enabled = true
  config.elb.ssl.certificates = ["arn:aws:acm:us-east-1:111111111111:certificate/11111111-2222-3333-4444-555555555555"]
end
```

You should must set up [SSL Support]({% link _docs/features/https-ssl-support.md %}) if you're using this.

{% include config/reference/header.md %}
{% include config/reference/elb.md %}
{% include config/reference/footer.md %}
