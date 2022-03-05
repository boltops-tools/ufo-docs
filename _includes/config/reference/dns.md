dns.domain | nil | This is the recommended option to set. The `dns.hosted_zone_name` and `dns.name` options conventionally uses this. This setting alone is enough to activate ufo adding the route53 DNS record.
dns.hosted_zone_id | nil | The hosted zone id. Takes precedence over `dns.hosted_zone_name`. This is useful for split-horizon DNS.
dns.hosted_zone_name | nil | The hosted zone name. Setting `dns.domain` is preferred over `dns.hosted_zone_name`.
dns.name | nil | The domain name. IE: my.domain.com. However, setting `dns.domain` is preferred over `dns.name`, so the name can conventionally set to something like `demo-web-dev.domain.com`. You then manually CNAME your pretty domain to it. IE: `www.domain.com` -> `demo-web-dev.domain.com`. This gives you more control over the user-friendly DNS.
dns.ttl | 60 | DNS TTL
dns.type | CNAME | DNS Type