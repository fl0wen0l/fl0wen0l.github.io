---
title: "Week 9 Worklog"
date: "2025-10-04"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---




### Overview

Week 9 covers Amazon CloudWatch: metrics, logs, alarms, and dashboards. This week follows the "AWS CLOUDWATCH WORKSHOP" at https://000008.awsstudygroup.com/ and focuses on collecting observability data, creating alarms, and building dashboards for troubleshooting and cost-conscious monitoring.

### Objectives

- Understand CloudWatch metrics, logs, alarms, and dashboards.
- Instrument applications (or simulate metrics) and send custom metrics to CloudWatch.
- Create alarms and dashboards to monitor system health and trigger actions.
- Practice cleaning up CloudWatch resources to avoid unnecessary charges.

### Prerequisites

- AWS account with permissions to create CloudWatch metrics, logs, alarms, and dashboards.
- Optional: sample application or scripts that emit metrics/logs (can use `awscli` to push test metrics).

### Tasks to be carried out this week:
| Day | Task                                                                                                              | Start Date | Completion Date | Reference Material                                          |
| --- | ----------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------------------------- |
| 1   | Workshop overview & preparation: review CloudWatch concepts and confirm IAM permissions.                          | 10/04/2025 | 10/04/2025      | <https://000008.awsstudygroup.com/>                         |
| 2   | Preparatory steps: create or identify an instance/app that emits metrics/logs; create a log group if needed.      | 10/05/2025 | 10/05/2025      | <https://000008.awsstudygroup.com/2-preparatory-steps/>     |
| 3   | CloudWatch Metrics: view built-in metrics, push custom metrics (using `put-metric-data`), and explore namespaces. | 10/06/2025 | 10/06/2025      | <https://000008.awsstudygroup.com/3-cloud-watch-metric/>    |
| 4   | CloudWatch Logs: create log groups/streams, push test logs, and query logs with CloudWatch Logs Insights.         | 10/07/2025 | 10/07/2025      | <https://000008.awsstudygroup.com/4-cloud-watch-logs/>      |
| 5   | CloudWatch Alarms: create alarms for important metrics and test alarm states (OK → ALARM → INSUFFICIENT_DATA).    | 10/08/2025 | 10/08/2025      | <https://000008.awsstudygroup.com/5-cloud-watch-alarm/>     |
| 6   | CloudWatch Dashboards: build a dashboard that aggregates key metrics and logs for the application.                | 10/09/2025 | 10/09/2025      | <https://000008.awsstudygroup.com/6-cloud-watch-dashboard/> |
| 7   | Cleanup: delete test metrics, log groups, alarms, and dashboards created during the exercises.                    | 10/10/2025 | 10/10/2025      | <https://000008.awsstudygroup.com/7-clean-up-resources/>    |

### Week 9 Achievements

- Viewed and analyzed built-in CloudWatch metrics and pushed custom metrics for application testing.
- Created log groups and used CloudWatch Logs Insights to run queries and identify issues.
- Defined alarms for critical metrics and validated alert state transitions.
- Assembled a CloudWatch dashboard that surfaces application health and performance indicators.
- Cleaned up all CloudWatch test artifacts.

### Quick Reference (CLI examples)

Note: replace resource names and regions with your own values.

```bash
# Put a custom metric to CloudWatch
aws cloudwatch put-metric-data --metric-name PageViews --namespace "FCJ/Application" --value 1 --unit Count --region us-east-1

# Create a log group
aws logs create-log-group --log-group-name "/fcj/app/logs" --region us-east-1

# Put a log event (requires sequence token for persistent streams; good for testing)
aws logs put-log-events --log-group-name "/fcj/app/logs" --log-stream-name "test-stream" --log-events timestamp=$(date +%s)000,message="Test log entry" --region us-east-1

# Create a CloudWatch alarm
aws cloudwatch put-metric-alarm --alarm-name HighCPUAlarm --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 60 --threshold 80 --comparison-operator GreaterThanThreshold --evaluation-periods 2 --alarm-actions arn:aws:sns:us-east-1:123456789012:MyTopic --dimensions Name=InstanceId,Value=i-0123456789abcdef0 --region us-east-1

# Create a dashboard from a JSON body file
aws cloudwatch put-dashboard --dashboard-name FCJ-App-Dashboard --dashboard-body file://dashboard.json --region us-east-1

# List alarms
aws cloudwatch describe-alarms --region us-east-1

# Delete a dashboard
aws cloudwatch delete-dashboards --dashboard-names FCJ-App-Dashboard --region us-east-1

# Delete a log group
aws logs delete-log-group --log-group-name "/fcj/app/logs" --region us-east-1
```

### References

- AWS CloudWatch workshop: <https://000008.awsstudygroup.com/>
- Modules: Introduction, Preparatory steps, CloudWatch Metrics, CloudWatch Logs, CloudWatch Alarms, CloudWatch Dashboards, Cleanup resources

---

If you'd like, I can:
- Add detailed per-day CLI step-by-step instructions and example dashboard JSON.
- Provide CloudWatch Logs Insights example queries for common troubleshooting scenarios.
- Produce a Vietnamese translation of this page.


