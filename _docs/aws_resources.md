---
layout: default
category: Platforms - AWS
title: Resources
order: 3
permalink: /aws_resources.html
---

# AWS Resources, SDK Links, Tutorials, and Helpful Tidbits

## Avoid Charges For Common Services

After you have signed up for an AWS account and begin using services, keep in mind a few things to avoid unnecessary charges from  services that are not covered within Free Tier:

1. Make sure services that you are using are covered by the Free Tier. **If you use services that are not covered by the Free Tier, you will be charged for those services**. Before using a service, check [the list of Free Tier services](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc) to see if that service is covered.

2. [Set up Free Tier usage alerts](https://console.aws.amazon.com/billing/home#/preferences) to be notified in case you exceed your Free Tier usage limits. It's very simple, just enter your email and remember to keep an eye on your inbox to see if there's any usage alert from AWS.

3. It's a good idea to pay attention to your [Billing and Cost Management Console](https://console.aws.amazon.com/billing). This is where you can monitor your service usage to avoid exceeding service limits and unnecessary costs.

4. [Set up a cost budget](https://aws.amazon.com/getting-started/tutorials/control-your-costs-free-tier-budgets/). Since you won't be expecting any costs or charges, this step is optional. However, if you want to experiment with it, setting the cost budget to $0 or $1 to keep your cost as low as possible.

5. Each service you use has associated resources that should be cleaned up after you're done using the service. If a service is left running for a long period of time, the free limit of that service will be exceeded and you will be charged for it. Below are some common services/resources that may incur charges if not cleaned up properly after used. See the [AWS documentation](https://aws.amazon.com/documentation/) for a complete list of services in use and how to clean them up. Links for deleting or de-allocating a few common products are listed here:

   * [Delete S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)
   * [Terminate EC2 instances -- leaving EC2 instances running is a common cause of unexpected recurring charges](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)
   * Delete database tables: [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/getting-started-step-8.html), [RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_DeleteInstance.html), and/or [Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#delete-cluster)

The list above should be sufficient to get you started. You **do not** have to follow every step. However, pay close attention to **1, 3, and 5** because those are some of the most common solutions.

For additional information, AWS has a [comprehensive guide](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html) to help you avoid unexpected charges if you find yourself in an uncharted territory.



# Video Tutorials To Help You Get Familiar With AWS Cloud services

## Static Website Hosting on S3, Route 53 Domain Name Registration
In this AWS S3 tutorial the presenter walks step-by-step through the process of hosting a website on Amazon S3. The tutorial also includes setting up a custom domain name in AWS Route 53 and associating it with the website in S3.
(Note: domain registration is not free but many custom domains are available at an affordable price.)

[S3 Video Tutorial Available Here ] (https://www.youtube.com/watch?v=P_B79xXf-w8)

## AWS Lambda & DynamoDB | AWS Serverless Tutorial
This tutorial as uses AWS Lambda, AWS API Gateway and DynamoDB to create a serverless backend for your application. This is a three
part series that can be accessed on "Cloud Path" via youTube.

[Lambda DynamoDB Video Tutorial Available Here] (https://www.youtube.com/watch?v=VGerk8hrP9U)
[Reserved Words In DynamoDB] (https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ReservedWords.html)

## AWS Lamda Tutorial | Use AWS CloudWatch Events to Schedule a Lambda Function Invocation
In this AWS Lambda tutorial we set up a basic hello world Lambda function that's triggered by a scheduled CloudWatch event to run every minute. This video is part of the AWS Serverless tutorial series.

[Lambda & CloudWatch Video Tutorial Available Here.](https://www.youtube.com/watch?v=-v4LMV5DAD4&list=PLD_RqipW0-9s-u1HXTglYV8Aam-5P3XLi&index=7&t=0s)

## How to Create EC2 Instance in AWS Step-by-step
Create Your EC2 Resources and Launch Your EC2 Instance.
How to create EC2 instance in AWS step by step.

[EC2 Video TutorialAvailable here](https://www.youtube.com/watch?v=1x5dWRaLvIw)

## AWS EC2 Linux Launch & Connection Using SSH

[EC2 Linux Launch Video Available Here] (https://www.youtube.com/watch?v=v0g1M5bb9u4)
[EC2 connect with Putty(Windows)] (This video will show how to use a PuTTY private key to connect to your Amazon EC2 Linux instance.)
