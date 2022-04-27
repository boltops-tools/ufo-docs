---
title: UFO Managed Security Groups
nav_text: Managed Security Groups
category: more
order: 2
---

UFO can create and manage two security groups. One for the ELB and one for the ECS tasks. Here are some considerations for these security groups:

* Network load balancers do not support security groups. So an ELB security group is only created when an Application ELB is used.
* The ECS security group for tasks is created only if network_mode awsvpc is used in the ECS task definition. This is because in bridge network mode the EC2 container instance’s Ethernet card and its security group is used. So there's no point in creating an ECS Security Group.
* In bridge mode, the EC2 containers group security group is outside the control of ufo. You’ll need to configure the security group appropriately yourself. UFO will only create and manage an ECS security group when awsvpc node mode is used, and ufo has control of the security group.
* The Managed ECS Security Group is automaticaly disabled when using bridge network mode.

## EC2 Instance Security Group Help

If you see that the Targets in the ELB Target Group are reporting unhealthy, it is usually a security group issue.  You might see this output with `ufo ps`:

    $ ufo ps
    +----------+------+--------------+----------------+---------+-------------------------+
    |    Id    | Name |   Release    |    Started     | Status  |          Notes          |
    +----------+------+--------------+----------------+---------+-------------------------+
    | 070a9c0a | web  | demo-web:169 | 6 minutes ago  | STOPPED | Failed ELB health check |
    | d02728ba | web  | demo-web:169 | 3 minutes ago  | STOPPED | Failed ELB health check |
    | 8dcf81ae | web  | demo-web:169 | 13 seconds ago | RUNNING |                         |
    +----------+------+--------------+----------------+---------+-------------------------+
    There are targets in the target group reporting unhealthy.  This can cause containers to cycle. Here's the error:
    (service development-demo-web-Ecs-13D2BFA4ULNC9) (instance i-0812a3bcd94babf12) (port 32779) is unhealthy in (target-group arn:aws:elasticloadbalancing:us-east-1:111111111111:targetgroup/devel-Targe-1MJR8V6VOWBGI/3f44f85710fe0297) due to (reason Request timed out)
    Check out the ECS console events tab for more info.
    $

If you are using network mode bridge, then you'll need to need to configure the container instance's security group to allow traffic to the Docker ephemeral port range. For details on the Docker ephemeral port range on AWS, you search for "ephemeral" on the [ECS Port Mapping Docs](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_PortMapping.html).

In general, ports below 32768 are outside of the ephemeral port range. So an easy way to configure the container instance's security group is to whitelist ports 32768 to 65535 to your VPC's CIDR block. An example of a CIDR block range could be `10.0.0.0/16`. The CIDR block is being suggested because the Application load balancer and security group created by ufo can change if you change the load balancer settings.

If you are using a network load balancer and are running bridge network mode, you need to whitelist ports 32768 to 65535 to `0.0.0.0/0`.  This is because network load balancers operate at layer 4 of the OSI model and cannot be assigned security groups, so they use the instance's security group.  If you feel this is too loose of permissions, you can use awsvpc mode. There are some considerations for awsvpc mode though which is covered in [ECS Network Mode: bridge vs awsvpc]({% link _docs/more/ecs-network-mode.md %}).
