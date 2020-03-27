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

