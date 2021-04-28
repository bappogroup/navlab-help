# AWS Architecture

## Overview

We have two environments dev and prod in the same AWS account. Resources for dev with have a prefix of `dev-`. Some resources are shared between dev and prod which are highlighted.

## ECR \(shared\)

ECR is used to store docker images for the API. Each image corresponds to a commit in git and has a tag of the commit hash.

## Cognito

We have two Cognito user pools for dev and prod. We currently use Cognito for

* Authentication
* Email subscription

User attributes:

* `email`: Standard Cognito attribute. Also used as username for Cognito.
* `given_name`: Standard Cognito attribute. Must be in sync with `firstName` in Postgres.
* `family_name`: Standard Cognito attribute. Must be in sync with `lastName` in Postgres.
* `custom:_tenant`: Custom attribute. Must be in sync with `_tenant` in Postgres.
* `custom:subPublishHistory`: Custom attribute. Whether the user is subscribed to publish history emails.

## Front-end Assets

The front-end assets for the web app are served by Cloudfront which is managed by Amplify.

## Public Hosted Zone \(shared\)

We have one public hosted zone to configure DNS records for navlab.online.

* navlab.online and www.navlab.online: Prod web app served by Cloudfront.
* api.navlab.online: Prod API endpoint \(alias pointing to the ALB\).
* dev.navlab.online: Dev web app served by Cloudfront.
* api.dev.navlab.online: Dev API endpoint \(alias pointing to the ALB\).
* local.navlab.online: Pointing to localhost for local development.

## VPC

One for dev \(10.0.0.0/16\) and one for prod \(10.1.0.0/16\).

Each VPC has:

* two public subnets in two availability zones that route non-local traffic to the internet gateway.
* one internet gateway with an Elastic IP for the internet to access public subnets.
* two private subnets in two availability zones that route non-local traffic to the NAT gateway.
* one NAT gateway to enable resources in the private subnets to access the internet.

## Private Hosted Zone

We use private hosted zone to assign DNS names to resources in the VPC that are not public-facing \(e.g. database\) so that we can find them consistently even if their IP addresses change frequently. One for dev \(dev.internal\) and one for prod \(prod.internal\).

* db.dev.internal: CNAME record pointing to RDS.

## API ALB

The ALB is the entry point of our backend so it needs to be placed in a public subnet. It listens on the following ports:

* Port 80: Redirects to Port 443.
* Port 443: Forwards requests to the API ECS service. Please note that a container port is configured in the target group \(currently 4000\) to forward to the ECS service which must match the port defined in the task.

It also has a security group configured to only allow traffic from Port 80 and 443.

## API ECS

### Cluster

One ECS Cluster for dev and one for prod.

### Service

Currently only one ECS service in a cluster. If we want fast rollback we should use blue-green deployment which keeps the old version of the task running in another service until the new one is ready and we are confident that there is no error.

The ECS services have a security group configured to only allow traffic from the ALB.

Currently each service runs only one task. It's best to configure auto-scaling for production that automatically adjusts the desired count based on CPU and memory usage.

### Task

A task must have a task role and an execution role configured.

Task role: For application code to access AWS services like Cognito and SNS \(navlab-api-ecs-task-role shared by dev and prod\)

Execution role: For ECS runtime to pull images from ECR, log events to CloudWatch and pull secrets from Parameter Store \(ecsTaskExecutionRole shared by dev and prod\).

## RDS

RDS sits in our private subnets. The password is stored in Parameter Store.

## Bastion Host

We have an EC2 bastion host that sits inside the VPC. So if you need to connect to the database, you can first SSH to the bastion host and connect to the database from there.

The SSH key is stored in LastPass. SSH to the bastion host using this command:

```bash
ssh -i /path/to/pem ec2-user@3.24.161.105
```

## SES

Configured to send emails from `@navlab.online` which is intended to be used for sending system notifications to registered NavLab users \(sender is no-reply@navlab.online\). It's not for sending emails to external customers on behalf of NavLab users which should be done through Mailjet.

## DB Event SNS Topic

This topic is used to broadcast database events like `records_created`, `record_updated`and `records_deleted` so that we can extend our system without complicating the API code.

## Publish History Email Worker

This is a worker that listens for newly created publish histories and send email notifications to subscribed users.

It's a combination of an SQS queue which subscribes to the SNS topic and a lambda function that polls SQS and processes the messages.

