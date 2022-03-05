autoscaling.enabled | true | Turns ECS Service AutoScaling on or off.
autoscaling.max_capacity | 5 | AutoScaling maximum capacity.
autoscaling.min_capacity | 1 | AutoScaling minimum capacity.
autoscaling.predefined_metric_type | ECSServiceAverageCPUUtilization | The AutoScaling metric to use. For ECS, theses are ECSServiceAverageCPUUtilization and ECSServiceAverageMemoryUtilization. AWS Docs: [PredefinedScalingMetricSpecification](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-autoscalingplans-scalingplan-predefinedscalingmetricspecification.html)
autoscaling.scale_in_cooldown | nil | AutoScaling cooldown time when scaling in. The time in seconds to pause between scaling events.
autoscaling.scale_out_cooldown | nil | AutoScaling cooldown time when scaling out. The time on seconds to pause between scaling events.
autoscaling.target_value | 75.0 | AutoScaling Target Value.