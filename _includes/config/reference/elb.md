elb.default_actions | nil | Override the Listener default actions. This provides you a lot of control.
elb.enabled | auto | Enables creating the ELB. Can be "auto", true or false. Auto means will create an ELB when role is `web`.
{% include config/reference/elb-existing.md %}
elb.health_check_interval_seconds | 10 | Time, in seconds, between health checks.
elb.health_check_path | / | Health check url path.
elb.healthy_threshold_count | 3 | Number of health checks successes before considered healthy.
elb.port | 80 | ELB Listener port
elb.redirect.code | 302 | Redirection status code
elb.redirect.enabled | false | When set to true, the Listener redirect to HTTPS by default. You should also set up SSL.
elb.redirect.port | 443 | Redirection status port
elb.redirect.protocol | HTTPS | Redirection protocol
elb.ssl.certificates | / | The ACM certificates to use. Example: ["arn:aws:acm:us-west-2:11111111:certificate/EXAMPLE"]. If only using one cert, can also just provide a String instead of an Array. Remember to also set `ssl.enabled = true`
elb.ssl.enabled | false | Whether or not to enable the creationg of an SSL Listener. If enabled, `ssl.certificate` should be set.
elb.ssl.port | 443 | ELB SSL Listener port
elb.unhealthy_threshold_count | 3 | Number of health check failures before considered unhealthy.