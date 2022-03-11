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

With awsvpc network mode, you must consider the limit of ethernet cards for the instance type. If the instance supports ENI Trunking, then this limit is larger. However, if the instance does not support ENI Trunking, the ENI limit is extremely small.

For the ENI Trunking Task limits per instance: [Elastic Network Interface Trunking](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/container-instance-eni.html)

For example, an m5.large instance has a limit of 10 tasks per instance.
For EC2 instances that do not support ENI Trunking,
the table that lists the limits are under section the AWS EC2 docs under [IP Addresses Per Network Interface Per Instance Type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html)

For example, a t3.small instance has a limit of 3 ethernet cards. This means, at most, you can run 2 ECS tasks on that instance in awsvpc network mode, since the host already uses one network card.

In awsvpc mode, each ECS task gets its own dedicated network card. The advantage is more granular control of the permissions per ECS service. For example, when services A and B are using awsvpc mode, they can have different security groups associated with them. In this mode, ufo creates a security group and sets up the permissions so the load balancer can talk to the containers.  You can also add additional security groups using `.ufo/config.rb`.

A con of awsvpc network mode is that ECS doesn't seem to do a great job of surfacing errors when the limit is reached. This makes them pretty difficult to debug. See: [Debugging awsvpc ENI Limits]({% link _docs/debug/awsvpc-eni-limits.md %}).

The following table summarizes the pros and cons:

Network mode | Pros | Cons
--- | ---
bridge | The numbers of containers you can run will not be limited due to EC2 instance network cards limits. | Less fine grain security control over security group permissions with multiple ECS services.
awsvpc | Fine grain security group permissions for each ECS service. | The number of containers can be limited by the number of network cards the EC2 instance type supports.

## Suggestion

Think for most, bridge mode is good. Run the ECS containers on private subnets, if you're also ok to pay for the NAT Gateways. Though not as good as awsvpc, private subnets provide increased security isolation. Scaling goes much smoother with bridge mode. With awsvpc, you'll likely hit the ENI limit before hitting your own apps limits. It just depends on what bottlenecks first. Ironically, if you're app is slower, then it's actually good for scaling with awsvpc mode.  The app's limit will reach, and the ECS service and cluster will scale before hitting the ENI limit. So the ENI limit is not the bottleneck. Write slower code to it can scale 🤣  It's also easier not to have to debug ENI limits as ECS doesn't seem to surface the errors well.

Though, use awsvpc mode with ENI trunking supported instances if you have that hard requirement. Some companies must do so for compliance reasons. There are trade-offs here, and companies have different needs and may prefer to pay the extra costs to scale with awsvpc mode.
