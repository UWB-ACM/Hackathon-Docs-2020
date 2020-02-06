---
layout: default
category: Getting Started
title: Cloud Provider Setup
order: 5
permalink: /cloud_setup.html
---

# Getting Started with Cloud Providers

For this hackathon, you can create, test, and deploy your project on any 
cloud provider of your choice. This page has resources for account setup 
and billing considerations for the three main cloud providers:

* **Amazon Web Services** (AWS)
* **Microsoft Azure**
* **Google Cloud Platform** (GCP)

## Free Accounts

For this hackathon, the applications you create and host will most likely 
have low traffic and will not be deployed long-term, and the financial 
cost of your services will be limited. However, depending on 
the goals of your application, there may be a specific cloud platform 
which meets your needs and will incur the lowest financial cost for your 
team.

Two common needs and pain points for students are:

1. Receiving account credits
2. Requiring a credit card to be on file

Let's compare and contrast the costs and billing requirements of the three primary 
cloud providers.

| Provider | Free Services | Free Services for Students |
| -------- | ------------- | -------------------------- |
| Amazon Web Services | AWS has a [Free Tier](https://aws.amazon.com/free/) for most of their common services, which meets most common needs for prototyping applications and small-scale deployment. ***No platform credits are provided on the free tier by default.*** | AWS has a student program called [AWS Educate](https://aws.amazon.com/education/awseducate/), which provides a $35 platform credit and ***does not require a credit card to register***. 
| Google Cloud Platform | GCP has a [Free Tier](https://cloud.google.com/free/); many common services are free to use and ***new customers get a $300 platform credit***. | GCP has an [online learning platform](https://edu.google.com/programs/?modal_active=none) which provides free courses to students and faculty. For this hackathon, it isn't a suitable platform. |
| Microsoft Azure | Azure has a [Free Tier](https://azure.microsoft.com/en-us/free/); many common services are free to use and ***new customers get a $200 platform credit***. | Azure offers [a special platform credit for students](https://azure.microsoft.com/en-us/free/free-account-students-faq/); ***a $100 platform credit is included and registration for Azure for Students does not require a credit card on file***. |

## Platform Credits

Besides obtaining platform credits directly from the providers (as is the case 
with [GCP](https://cloud.google.com/free/) and [Azure](https://azure.microsoft.com/en-us/free/)),
there are several resources to get account credits and eliminate out-of-pocket 
costs initially.

First and foremost, use your `.edu` email address to register for the [GitHub Education 
Pack](https://education.github.com/pack). GitHub's corporate partners provide 
benefits and credits that are not accessible elsewhere; this includes free access 
to AWS Educate and $100 of additional Azure credits. The Education Pack also has 
amazing benefits with other companies, such as a $50 account credit for Digital Ocean, 
a popular VM provider.

## Account Creation & Setup

Begin the registration process for each provider's free tier at the following
pages:

* [Azure for Students](https://azure.microsoft.com/en-us/free/free-account-students-faq/); click `Activate` to begin the registration process
* [Azure](https://azure.microsoft.com/en-us/free/); click `Start free`
* [Google Cloud Platform](https://cloud.google.com/free/): click `Get started for free`
* [AWS Educate](https://www.awseducate.com/registration#APP_TYPE); select `Student` and fill out the web form to apply for student benefits. Make sure to use your `.edu` email address.
* [AWS](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc); click `Create A Free Account`

Note that Azure for Students and AWS Educate may take several business days to be set 
up; applying for access to these platforms 2 weeks before the hackathon is recommended 
if you don't want to use a standard free tier account during the event.

## Cost Management

For enterprise-level applications, there can be significant monthly bills 
from cloud providers. Typically these applications produce millions of 
service activities from thousands of users, and the organization hosting 
these applications have a business model to support the costs.

As students, our needs are substantially different from those of a 
company with seed funding and revenue streams. Even so, it is easy to burn 
through a substantial amount of money without realizing it.

### Culprits of Unexpected Costs

Each user's needs will be different, but there are a few common gotchas 
which tend to inflate AWS costs: 

* Unused or underutilized virtual machines (EC2 instances)
* Unused or underutilized peripherals for EC2 instances (static IP reservations, storage volumes)
* Data transfer costs associated with EBS or S3

The primary lessons here are to clean up resources which you don't need 
and research the costs of services which you plan on utilizing heavily. 
[Read more about the 'gotchas' here.](https://www.itproportal.com/features/7-hidden-aws-costs-that-could-be-killing-your-budget/)

Many of AWS's hidden or unexpected costs also apply to GCP and Azure. 

### Preventing Unexpected Costs

At scale, GCP seems to be the friendlier platform for cost management than AWS, 
and they have significant advantages in specific service niches.
GCP also allows you to set budgets and monitor cost utilization 
[in a more granular fashion](https://cloud.google.com/billing/docs/how-to/budgets) than 
[AWS billing monitoring](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html).

 For enterprise systems and corporate customers, 
[Azure may provide advantages and disadvantages](https://www.informationweek.com/cloud/infrastructure-as-a-service/hidden-cloud-costs-aws-azure-management-costs-compared/a/d-id/1298029) 
depending on the existing system configuration, but for students, the 
difference is likely not noticable.

