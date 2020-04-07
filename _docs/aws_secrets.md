---
layout: default
category: Platforms - AWS
title: Secrets & Permissions
order: 2
permalink: /aws_secrets.html
---

# AWS Secrets & Permissions Management

## AWS Identity and Access Management (IAM)

After you have created your AWS account, it will become your root account. A root account provides unlimited access to everything. However, having that type of access can create many security risks. When working in a group, it is best practice to have multiple users with limited access under one account. By doing this, it can make collaboration easier and lower any security risk.

The purpose of AWS IAM is to help the root user manage other IAM users and their varying levels of access to AWS resources. For example, AWS users can be created and assigned individual access to services which best fit their role in the group.

AWS IAM is free to use and it is recommended for security reasons.

To create IAM users, the root user must be logged in first then [click here](https://console.aws.amazon.com/iam/home#/home).

Next follow these steps to create one or multiple IAM users depending on the group size:

1. Click on Users, Add Users

   ![step1](/assets/iam_step1.png)


2. Enter a descriptive name for an IAM user, check both Access Type boxes, create a password as in the snippet below, then click next

![step2](/assets/iam_step2.png)


3. Follow the snippet below to grant the user appropriate permissions, then click next

![step3](/assets/iam_step3.png)


4. This step is optional, you can fill out the fields or click next 

![step4](/assets/iam_step4(optional).png)


5. Finally, review your information and click Create User. 

**Remember to download the csv file so you have all your information like username and password that you won't forget later**

![step5](/assets/iam_final.png)


***
Note that doing this is helpful if you have private information like credit card number that you used when creating your AWS account. Instead of sharing your root account with your group members creating IAM users will help protect your private information while it's still letting you and your groupmates have full control to services so everybody can work together. 
***

