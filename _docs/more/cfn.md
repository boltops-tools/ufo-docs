---
title: Customizing CloudFormation Resources
nav_text: Cfn
categories: more
order: 1
---

Ufo allows you to customize the CloudFormation template with the ECS resources by merging in your own custom YAML.

The properties in the file `.ufo/config/cfn/dev.yml` map directly to ufo's CloudFormation resources. Here's a list of the resources that you can customize:

* Dns
* EcsSecurityGroup
* EcsSecurityGroupRule
* EcsService
* Elb
* ElbSecurityGroup
* ExecutionRole
* Listener
* ListenerSsl
* ScalingPolicy
* ScalingRole
* ScalingTarget
* TargetGroup
* TaskDefinition
* TaskRole

Refer to the source code for the latest list: [stack/builder/resources.rb](https://github.com/boltops-tools/ufo/blob/master/lib/ufo/cfn/stack/builder/resources.rb)

## Customization Example

Let's customize the `AWS::ElasticLoadBalancingV2::TargetGroup` resource created by CloudFormation.  We'll adjust the `deregistration_delay.timeout_seconds` to `8`.  Here's the relevant section

.ufo/config/cfn/base.yml

```yaml
TargetGroup:
...
  TargetGroupAttributes:
  - Key: deregistration_delay.timeout_seconds
    Value: 8
```

The value will be merged to the generated CloudFormation template under the corresponding "TargetGroup Properties".  The generated template looks something like this:

```yaml
TargetGroup:
  Properties:
...
    TargetGroupAttributes:
    - Key: deregistration_delay.timeout_seconds
      Value: 8
...
```


In this way, you can customize and override any properties associated with resources created by the ufo CloudFormation stack.

## Layering Support

The `base.yml` file always get evaluated, and env-specific YAML file is layered or merged together. This minimizes duplication.  For example, these files are merged for `UFO_ENV=dev`.

    .ufo/config/cfn/base.yml
    .ufo/config/cfn/dev.yml

Note this feature is available in v6+.
