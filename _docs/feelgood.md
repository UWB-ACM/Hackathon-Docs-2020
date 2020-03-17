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
    * Invokes the [Simple Notification Service](https://aws.amazon.com/sns/) SDK
    * Makes a `subscribe()` call to SNS, passing the user's data and topic ARN
* SNS saves the user's phone number as a subscriber under the specified topic.
* If the Lambda function processes successfully, it will return a success code 
  to the API Gateway method, which in turn returns the HTTP 200 status to the 
  user's browser. The Lambda function also publishes a text message to the user's 
  provided number, which confirms subscription and welcomes the user.

[View the website code here.]()

[View the subscription Lambda function here.]()

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

[View the publication Lambda function here.]()

Here's a diagram of the subsystem's flow of logic:

![FeelGood publication mechanism](/assets/feelgood_pub.png)
  
## Implementation Notes

The specific instructions, how-tos, and details we encountered are organized 
here by AWS product.

### S3 & Website Construction

More info coming soon.

### API Gateway

More info coming soon.

### Lambda

More info coming soon.

### SNS

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
