---
layout: post
title: "Amazon Lambda"
description: "Some notes about Amazon Lambda ans Serverless"
comments: true
keywords: "serverless"
---

#### Provisioned Concurrency

[AWS Lambda Provisioned Concurrency](https://www.serverless.com/blog/aws-lambda-provisioned-concurrency/)

#### Configuring a Lambda function to access resources in VPC

Connect your function to private subnets to access private resources. If your function needs internet access, use NAT. 
Connecting a function to public subnets does NOT give it internet access or a public IP address

When you connect to VPC, Lamda creats an Elastic Network Interface (ENI)
Multiple functions connected to the same subnets share network interfaces

Permissions needed:
- `ec2:CreateNetworkInterface`
- `ec2:DescribeNetworkInterfaces`
- `ec2:DeleteNetworkInterface`

These persmissions included in `AWSLambdaVPCAccessExecutionRole` managed policy.


#### Synchronous invocation

```bash
$ aws lambda invoke --function-name my-function --payload '{ }' response.json
```
![lambda synchronous invocation](https://docs.aws.amazon.com/lambda/latest/dg/images/invocation-sync.png)

#### Asynchronous invocation

![asynchronous invocation](https://docs.aws.amazon.com/lambda/latest/dg/images/features-async.png)

```bash
$ aws lambda invoke --function-name my-function --invocation-type Event --payload '{"key": "value"}' response.json
{
  "StatusCode": 202
}
```
If your function returns an error, AWS will automatically retry the invoke twice, for a total of three invocations.

`Dwell time` how long your request spent in the service queue

#### Poll-Based Invocation
- Amazon Kinesis
- Amazon SQS
- Amazon DynamoDB

![invocation types](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2019/07/02/table-invoke-lambda3.png)

#### Function scaling
![function scaling](https://docs.aws.amazon.com/lambda/latest/dg/images/features-scaling.png)

#### Function scaling with Provisioned Concurrency

![provisioned-concurrency](https://docs.aws.amazon.com/lambda/latest/dg/images/features-scaling-provisioned.png)

#### Autoscaling with Provisioned Concurrency

![provisioned-concurrency](https://docs.aws.amazon.com/lambda/latest/dg/images/features-scaling-provisioned-auto.png)
