# AWS CloudWatch

## Push Custom Metric

`aws cloudwatch put-metric-data --metric-name FunnyMetric --namespace Custom --value 1234 --dimensions InstancId=i-22342,InstancType=m5.large --profile aws-profile --region us-east-2`

## Export Metrics

`aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=i-234123 --statistics Maximum --start-time 2020-11-01T00:00:00 --end-time 2020-11-30T00:00:00 --period 360 --profile aws-profile --region us-east-2`
