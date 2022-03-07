---
title: "ECS Network Mode: bridge vs awsvpc"
nav_text: ECS Network Mode
category: more
order: 1
---

## Pros and Cons: bridge network mode

With network bridge mode, the Docker containers of multiple services share the EC2 container instance's security group. So you have less granular control over opening ports for specific services only. For example, let's say services A and B are both configured to use bridge network mode. If you open port 3000 for service A, it will also open port 3000 for service B because they use the **same** security group at the EC2 instance level.

One advantage of bridge mode is that you can use dynamic port mapping and not worry about network card limits.

## Pros and Cons: awsvpc mode

With awsvpc network mode, you must consider the limit of ethernet cards for the instance type. If the instance supports ENI Trunking, then this limit is decently large. However, if the instance does not support ENI Trunking, the ENI limit is small.

For the ENI Trunking Task limits per instance: [Elastic Network Interface Trunking](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/container-instance-eni.html)

For example, an m5.large instance has a limit of 10 tasks per instance.
For EC2 instances that do not support ENI Trunking,
the table that lists the limits are under section the AWS EC2 docs under [IP Addresses Per Network Interface Per Instance Type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html)

For example, a t3.small instance has a limit of 3 ethernet cards. This means, at most, you can run 2 ECS tasks on that instance in awsvpc network mode, since the host already uses one network card.

In awsvpc mode, each ECS task gets its own dedicated network card. The advantage is more granular control of the permissions per ECS service. For example, when services A and B are using awsvpc mode, they can have different security groups associated with them. In this mode, ufo creates a security group and sets up the permissions so the load balancer can talk to the containers.  You can also add additional security groups using `.ufo/config.rb`.

The following table summarizes the pros and cons:

Network mode | Pros | Cons
--- | ---
bridge | The numbers of containers you can run will not be limited due to EC2 instance network cards limits. | Less fine grain security control over security group permissions with multiple ECS services.
awsvpc | Fine grain security group permissions for each ECS service. | The number of containers can be limited by the number of network cards the EC2 instance type supports.

## Suggestion

Think for most bridge mode is good. Run the ECS containers on private VPCs and only whitelist to your VPC CIDR. Scaling will go much smoother with this setup. Though, use awsvpc mode with ENI trunking supported instances if you have that requirement. Ultimately, companies have different requirements and may prefer to pay the extra costs that come with awsvpc mode.
