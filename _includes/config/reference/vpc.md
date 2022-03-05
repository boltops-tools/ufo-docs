vpc.id | nil | Used to create ecs and elb security groups in the CloudFormation template.
vpc.subnets.ecs | nil | The subnets the ECS Container Instances are on. So this is where you want your containers to run.
vpc.subnets.elb | nil | Subnets used by the ELB load balancer.  Defaults to same subnets as ECS subnets when not set.
vpc.security_groups.ecs | nil | Additional security groups to associate with the ECS tasks.
vpc.security_groups.elb | nil | Additional security groups to associate with the ELB.
vpc.security_groups.managed | true | Whether or not to create managed security groupsIf you disable, then no [managed security groups]({% link _docs/more/security-groups.md %}) will be created by UFO.