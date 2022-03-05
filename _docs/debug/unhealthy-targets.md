---
title: Debugging Unhealthy Targets
nav_text: Unhealthy Targets
category: debug
order: 1
---

If the ECS Load Balancer keeps reporting that your app has unhealthy targets and is cycling:

    draining -> initial -> draining

You'll see `ufo ps` output that looks something like this:

    $ ufo ps
    +----------+------+--------------+----------------+---------+-------------------------+
    |    Id    | Name |   Release    |    Started     | Status  |          Notes          |
    +----------+------+--------------+----------------+---------+-------------------------+
    | d02728ba | web  | demo-web:169 | 3 minutes ago  | STOPPED | Failed ELB health check |
    | 8dcf81ae | web  | demo-web:169 | 13 seconds ago | RUNNING |                         |
    +----------+------+--------------+----------------+---------+-------------------------+
    There are targets in the target group reporting unhealthy.  This can cause containers to cycle. Here's the error:
    (service development-demo-web-Ecs-13D2BFA4ULNC9) (instance i-0812a3bcd94babf12) (port 32779) is unhealthy in (target-group arn:aws:elasticloadbalancing:us-east-1:111111111111:targetgroup/devel-Targe-1MJR8V6VOWBGI/3f44f85710fe0297) due to (reason Request timed out)

This page has some debugging steps that may help.

## Increase Health Check Interval

The first thing you should probably do is increase the "Health check settings / Interval".  Otherwise, the Load Balancer will keep killing the containers, which can be pretty frustrating.

Remember to revert the Interval after successfully getting your targets to register as healthy.

## Confirm HTTP Success Code

Make sure your app is returning a 200 HTTP Success response code. Using verbose mode with curl can be helpful:

    curl -v URL:PORT

If your ECS tasks are running on private subnets, you'll need to hop onto an instance that has access. The container instance that is running the docker container is good.

If the app is returning 200 Success Codes, the issue likely has to do with security groups.

## Security Groups

The security group associated with the ECS task or container needs to be allowed access for the Load Balancer to see it as healthy. It can be tricky to identify the security group because it depends on your ECS configuration.

### network mode

Depending on the [Network Mode](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html#network_mode) your ECS task is configured with, you'll need to check different security groups.  You can check your ECS Task on the EC2 console to find the "Network Mode". The most common network modes used are bridge and awsvpc.

### network mode: bridge

If using `networkMode=bridge`, which is the default for ECS EC2, then you should check the security group of the **EC2 Instance**.

For `networkMode=bridge`, you can open up all Docker ephemeral ports so the Load Balancer can check the health status of the docker containers. The Docker ephemeral ports are 49153 through 65535. Usually, a security group inbound rule that allows port 49153-65535 to the VPC CDIR range is good.

In general, ports below 32768 are outside of the ephemeral port range. So an easy way to configure the container instance's security group is to whitelist ports 32768 to 65535 to your VPC's CIDR block. See: [ECS Port Mapping Docs](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_PortMapping.html). An example of a CIDR block range could be `10.0.0.0/16`.

### network mode: awsvpc

If using `networkMode=awsvpc`, you should check for the security group associated with the ECS Service, which in turn, is associated with the **ECS task**.

For `networkMode=awsvpc`, you can open up the ECS Service security group to allow the security group that the Load Balancer uses.

For this, it depends on the Load Balancer type you're using.

For **Application Load Balancers**, you can allow its security group access to the ECS security group. Usually, an inbound rule on the ECS Security Group that allows the ELB Security Group to talk to all ports is an excellent and clean setup.

    ELB Security Group -> ECS Security Group

## Network Load Balancers

For **Network Load Balancers**, you allow access with the **EC2 Instance** security group. This may be weird to some, but it's the EC2 Instance security because of how Network Load Balancers work. Network Load Balancers operate at OSI Network Layer or Layer 3. Security groups are not supported at this layer, so the Network essentially uses the same security group as the EC2 instances. Network Load balancers also transparently pass the client IP address straight to what the ECS tasks see. It's as if the client is directly talking go the ECS task without a Load Balancer in front. Thus you may need to whitelist the ECS security group ports 32768-65535 to `0.0.0.0/0` since the client IP address is what it sees.
