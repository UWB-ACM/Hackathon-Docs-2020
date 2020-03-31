---
layout: default
category: Projects
title: FeelGood
order: 1
permalink: /feelgood.html
---

# FeelGood, an Example Project for AWS

The hackathon organizers wanted to build an application that facilitates 
self-service for good vibes and positivity.

From that ideal, the project grew to provide additional registration 
options to facilitate laughs and silliness.

Read about the project and how we built it!

## Goals

* **Text Messages**: uplifting content, delivered straight to your phone.
* **Mix and Match Subscriptions**: users should be able to register for 
  message streams that fit their personality.
* **Fully Cloud Hosted**: The end-to-end deployment should be fully hosted 
  in the cloud, without running a VM or using local computing resources.

TL;DR: `automatic, supersonic, hypnotic, funky fresh`

## User Experience

1. A user goes to the application's website to sign up for messages.
   <div style="width:100%;height:0;padding-bottom:56%;position:relative;"><iframe src="https://giphy.com/embed/fQZX2aoRC1Tqw" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/fQZX2aoRC1Tqw">via GIPHY</a></p>

2. The user subscribes to a topic.
    <div style="width:100%;height:0;padding-bottom:75%;position:relative;"><iframe src="https://giphy.com/embed/mgqefqwSbToPe" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/mrw-joke-advice-mgqefqwSbToPe">via GIPHY</a></p>

3. The user gets a welcome message after subscribing to a topic of their choice. 
   They can continue to subscribe to other topics they are interested in.
    <div style="width:100%;height:0;padding-bottom:100%;position:relative;"><iframe src="https://giphy.com/embed/10a9ikXNvR9MXe" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/john-boyega-10a9ikXNvR9MXe">via GIPHY</a></p>

4. At set times during the day, the user gets a text message reminding them to:
    * Feel good
    * Drink water
    * Laugh
    * Commune with their crow friends
    
    Of course, the messages they actually get are dependent on which topics 
    they subscribed to.

    <div style="width:100%;height:0;padding-bottom:60%;position:relative;"><iframe src="https://giphy.com/embed/QBd2kLB5qDmysEXre9" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/time-mr-bean-look-at-the-QBd2kLB5qDmysEXre9">via GIPHY</a></p>
    <div style="width:100%;height:0;padding-bottom:113%;position:relative;"><iframe src="https://giphy.com/embed/wqd5THxOO39KM" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/laughing-the-big-bang-theory-wqd5THxOO39KM">via GIPHY</a></p>

## Architecture

To realize these desired behaviors & user experiences, we used AWS services 
and deployed the application to the cloud. The application's design is 
described below.

### User Subscription

* An [S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) 
  is used to host the subscription site. We configured 
  [domain redirection for a custom domain](https://www.freecodecamp.org/news/simple-site-hosting-with-amazon-s3-and-https-5e78017f482a/) 
  and SSL certificates for HTTP/S traffic.
* The user interacts with the site as they would any other.
* Upon clicking the `Submit` button, a JavaScript function makes an 
  [HTTP POST](https://en.wikipedia.org/wiki/POST_%28HTTP%29) request 
  to the application's API, sending the user's data from the website's 
  subscription form. The API endpoint is connected to a Lambda function using  
  [API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-getting-started-with-rest-apis.html).
* The Lambda function takes the JSON payload delivered by the POST request, 
  which contains the user's phone number and selected topic. The function 
  then:
    * Validates the phone number for correct formatting 
    * Looks up the [Amazon Resource Name](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) 
      (ARN) for the selected topic
    * Invokes the [Simple Notification Service](https://aws.amazon.com/sns/) (SNS) SDK
    * Makes a `subscribe()` call to SNS, passing the user's data and topic ARN
* SNS saves the user's phone number as a subscriber under the specified topic.
* If the Lambda function processes successfully, it will return a success code 
  to the API Gateway method, which in turn returns the HTTP 200 status to the 
  user's browser. The Lambda function also publishes a text message to the user's 
  provided number, which confirms subscription and welcomes the user.

[View the website code here.](https://github.com/UWB-ACM/feelgood/blob/master/website/)

[View the subscription Lambda function here.](https://github.com/UWB-ACM/feelgood/blob/master/subscribe/lambda_function.py)

Here's a diagram of the system's flow of logic:

![FeelGood subscription mechanism](/assets/feelgood_sub.png)

### User Notification

* To send messages out at regular intervals, we used CloudWatch Events 
  to create [rules which get invoked on a time schedule](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Scheduled-Rule.html).
* When creating the Event rule, the message publication function 
  [was selected as the target](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/RunLambdaSchedule.html).
* Upon being invoked, the Lambda function:
    * Chooses 4 messages to publish (one for each possible topic)
    * Looks up the ARN for each topic
    * Invokes the [Simple Notification Service](https://aws.amazon.com/sns/) SDK
    * Makes 4 `publish()` calls to SNS, where each call uses a different message and topic ARN
* SNS receives the SDK invocation to publish a message and distributes 
  the message for each topic to all subscribers for that topic. Each topic 
  is handled asynchronously.

[View the publication Lambda function here.](https://github.com/UWB-ACM/feelgood/blob/master/publish/lambda_function.py)

Here's a diagram of the subsystem's flow of logic:

![FeelGood publication mechanism](/assets/feelgood_pub.png)
  
## Implementation Notes

The specific instructions, how-tos, and details we encountered are organized 
here by AWS product.

### S3 & Website Construction

We used to the following resources as a guide for configuring S3 buckets to host the subscription website:

- [Setting Up a Static Website Using a Custom Domain Name Registered with Route 53](https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html)<br>
  This guide walks through how to register a custom domain using Route 53 and create/configure two S3 buckets to serve as containers for the contents of your website.

- [Setting up a Static Website](https://docs.aws.amazon.com/AmazonS3/latest/dev/HostingWebsiteOnS3Setup.html)<br>
  If you don't wish to buy a custom domain, use this guide instead. Your website will still be publicly accessible with a link that resembles the following: `http://your-bucket-name.s3-website-your-aws-region.amazonaws.com`

- [Obtaining an SSL Certificate for a Custom Domain](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-serve-static-website/)<br>
  This guide explains how to use CloudFront to request an SSL certificate for your custom domain in order to serve content over an HTTPS protocol. Make sure to scroll down to the section titled `Using a website endpoint as the origin with anonymous (public) access allowed`, which aligns with the configuration outlined in the two previous S3 setup guides.

- [Updating Existing Content with a CloudFront Distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/UpdatingExistingObjects.html)<br>
  If using CloudFront to serve your site over HTTPS, you will find updating the content of the site is not as simple as changing out the files in the S3 bucket due to CloudFront's caching properties. This guide outlines two workarounds: using versioned file names, and [invalidating the object](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html) within CloudFront which prompts it to reload objects the source.

- [Collecting and Submitting Form Data to Other AWS Services using HTML and jQuery AJAX requsts](https://aws.amazon.com/blogs/architecture/create-dynamic-contact-forms-for-s3-static-websites-using-aws-lambda-amazon-api-gateway-and-amazon-ses/)<br>
  The `Your “Contact Us” Form` section of this guide covers how to create a simple web form using HTML, and `Connecting it all Together` covers how to use [jQuery AJAX calls](https://api.jquery.com/jquery.ajax/) to send the data collected in the form to other AWS services to complete the subscription process.

### API Gateway

More info coming soon.

### Lambda

AWS Lambda handles all of FeelGood's computation. Within AWS Lambda, there are three important things that must be configured in order for FeelGood's code to operate correctly.

**Give Lambda access to SNS**
    By default, creating a Lambda function from scratch, adding SNS code, and pressing "Test". won't work. Why? Simple, because the Lambda function does not have *access* to SNS. Cloud systems operate on a least-required permissions model, meaning that in order for your Lambda function to access SNS, you must explicitly grant Lambda access to SNS (and any other AWS services you might want Lambda to access). Refer to [AWS documentation on IAM permissions for SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-using-identity-based-policies.html), and check out [the TL;DR explanation of IAM on the documentation site for this hackathon]({% link _docs/aws_secrets.md %}).
    
**Set up environment variables for Lambda functions**
    In order to keep the code looking clean, and to keep our Topic ARNs safe (more on these in the SNS section), we use environment variables to store the names of the SNS topics and their ARNs. These are exactly like environment variables on your computer. They're variables that you can access in your code like any other, but since they're not declared or otherwise modified in the source code, they're invisible and can't be accidentally leaked (say when you upload your code to a public git repository). After you create a Lambda function in the AWS portal, you will be greeted with the function's page. Here you can edit the code directly and modify the function's settings. Right below the code window, you will see a menu item for *Environment Variables*. Click Edit to modify them. The **key** is the variable name, and the **value** is the variable's value. Click Save to save these variables in the function. Now you can access the environment variables in the Lambda function's code by calling it using the key. See [AWS Documentation on Environment Variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) for more info.

**Set up aliases for Lambda functions**
    Lambda has a function called **Aliases**. Aliases are used in combination with function versions to manage what version of the Lambda function will be called by other AWS services. In FeelGood, aliases are used to connect API Gateway endpoints to the publish and subscribe Lambda functions. Under the *Actions* menu in the function, click on *Publish new version*, enter a description, and Publish it to create a new version. This new version will retain the functionality that it has at the time that it is published. Refer to the [AWS Docs on Lambda Versions](https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html) for more detail on what you can do with versioning. Aliases allow you to refer to different versions of a function with the same name. So for example in API Gateway you can link a GET request with a Lambda function using the alias "API_GET_Version". Then, no matter what function version you connect to that alias, be it 1, 10, or 1000, you will not need to change the API Gateway configuration when you update the Lambda code. You do however, need to modify which function version the alias will point to each time you publish a new version. You *can* alias to the $LATEST version in order to have Lambda automatically use whatever code exists when you Save the function, but this is not recommended. You should only alias to function versions that you have tested and verified to work. Further documentation on function aliases is available [here](https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html).
    
**Configure CloudWatch Trigger Invocation**
    In order for FeelGood messages to be sent out at a specific time, we use AWS CloudWatch events as a trigger to run the Lambda publish function at a specific time. Select an alias in the menu that you wish to trigger. In the *Designer* pane, there is a button called **Add trigger**. Clicking on that will bring up a drop down with a number of potential triggering services. FeelGood uses *CloudWatch Events/EventBridge*. Now AWS will present a menu to configure a rule. This rule will trigger the Lambda function. **Schedule expression** is where you define the time interval that the function should be triggered at. FeelGood uses cron syntax [(a good reference on cron syntax we found useful here)](https://www.adminschoice.com/crontab-quick-reference). Cron syntax uses UTC time. In FeelGood, the trigger uses the cron syntax **cron(0 18 * * ? *)**, which means every day at 10 AM PST. We recommend looking up a converter from UTC to your timezone. Saving and enabling this trigger will run the function at the specified time. AWS has a good explainer on creating CloudWatch Events Rules [here](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Scheduled-Rule.html).

### Simple Notification Service

More info coming soon.

### CloudWatch

More info coming soon.

## Gotchas & Lessons Learned

1. Amazon SNS may have reliability issues that are outside of your control; 
   *message delivery to subscribers is not guaranteed*. If high availability or 
   reliability is important to your application, consider incorporating 
   an API-accessible service which specializes in SMS, such as 
   [Twilio](https://www.twilio.com/).
2. Always, _always_, _**always**_ set up robust logging. AWS streamlines 
   setup of logging and performance monitoring for many services via 
   [CloudWatch](https://aws.amazon.com/cloudwatch/). This was particularly 
   useful when troubleshooting unreliable message delivery to subscribers, 
   monitoring S3 usage, and monitoring the frequency of API requests.

<div style="width:100%;height:0;padding-bottom:75%;position:relative;"><iframe src="https://giphy.com/embed/3KVChGOKcyyRO" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe></div><p><a href="https://giphy.com/gifs/steve-yahoo-retirement-3KVChGOKcyyRO">via GIPHY</a></p>
