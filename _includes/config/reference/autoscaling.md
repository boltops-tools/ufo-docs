autoscaling.enabled | true | Turns ECS Service AutoScaling on or off.
autoscaling.manual_changes.retain | true | Retain manual changes from `ufo scale` or with the AWS console. Note: Enabling means that `autoscaling.max_capacity` and `autoscaling.min_capacity` are only used for initial deployment.s
autoscaling.manual_changes.warning | true | UFO will show a warning about manual autoscaling changes. Set this to false to turn off this warning entirely.  Note: This warning will only appear `autoscaling.manual_changes.retain = false` has been set.
autoscaling.max_capacity | 1 | AutoScaling maximum capacity.
autoscaling.min_capacity | 1 | AutoScaling minimum capacity.
autoscaling.predefined_metric_type | ECSServiceAverageCPUUtilization | The AutoScaling metric to use. For ECS, theses are ECSServiceAverageCPUUtilization and ECSServiceAverageMemoryUtilization. AWS Docs: [PredefinedScalingMetricSpecification](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-autoscalingplans-scalingplan-predefinedscalingmetricspecification.html)
autoscaling.scale_in_cooldown | nil | AutoScaling cooldown time when scaling in. The time in seconds to pause between scaling events.
autoscaling.scale_out_cooldown | nil | AutoScaling cooldown time when scaling out. The time on seconds to pause between scaling events.
autoscaling.target_value | 75.0 | AutoScaling Target Value.
