---
layout: default
category: Projects
title: CrowFacts
order: 1
permalink: /crowfacts.html
---

# CrowFacts, an Example Project for AWS

More information coming soon!

## Goals

More information coming soon!

## User Experience

More information coming soon!

## Architecture

More information coming soon!

## Implementation Notes

More information coming soon!

### S3 & Website Construction

More information coming soon!

### API Gateway

There are multiple types of APIs that can be built using API Gateway, such as REST, HTTP, or WebSocket. **This document will focus specifically on REST APIs.** AWS' API Gateway documentation has a [helpful tutorial that covers creating a REST API with Lambda Integrations](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-getting-started-with-rest-apis.html). This was the approach taken for CrowFacts.

#### Create a Resource
A resource is something that can be accessed through the REST API using CRUD operations. GCP has a good [explanation of REST APIs and resources](https://cloud.google.com/apis/design/resources) if you are unfamiliar with the concept or would like to refresh your memory. In API Gateway, the resource also represents your endpoint. For example, on a hypothetical website for a car dealership, the URL might be `www.cars.com/buycar`. The endpoint in this scenario is `/buycar`. Each endpoint has its own set of methods.

#### Create a Method
A method is the actual operation to be performed on the resource, such as GET, PUT, POST, DELETE, etc. The method's settings are where integrations with Lambda functions including input pass-through are set. See the [API Gateway page on REST API methods](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-method-settings.html) for a detailed tutorial.

#### Setup CORS
CORS stands for Cross-Origin Resource Sharing. This is required to tell your browser that it is safe to access certain API endpoints. Whether or not CORS is required depends on what the API does. See [Mozilla's MDN Docs on CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) for more information on what it is and why it's needed. See [API Gateway Docs on Enabling CORS for REST APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html) for a tutorial on enabling CORS.

#### Deploy the API
Once everything is setup correctly, all that's left is to deploy the API. You can create **stages** like "staging" and "production" and deploy different versions on the API to each stage. It is recommended to deploy a staging version of the API for testing before deployed the production version of the API for use in an application. *Note that the name of the stage will be part of the endpoint URL*. See [API Gateway Docs on Deploying a REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-deploy-api.html) for a detailed walkthrough.

### Lambda

AWS Lambda handles all of CrowFacts' computation. Within AWS Lambda, there are several important things that must be configured in order for CrowFacts' code to operate correctly.

#### Give Lambda access to DynamoDB
By default, creating a Lambda function from scratch, adding DynamoDB code, and pressing "Test" won't work. Why? Simple, because the Lambda function does not have *access* to DynamoDB. Cloud systems operate on a least-required permissions model, meaning that in order for your Lambda function to access DynamoDB, you must explicitly grant Lambda access to DynamoDB (and any other AWS services you might want Lambda to access). Refer to [AWS documentation on IAM permissions for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/authentication-and-access-control.html), and check out [the TL;DR explanation of IAM on the documentation site for this hackathon]({% link _docs/aws_secrets.md %}).
    
#### Set up environment variables for Lambda functions
- In order to keep the code looking clean, and to keep our Topic ARNs safe (more on these in the DynamoDB section), we use environment variables to store the names of the DynamoDB topics and their ARNs. These are exactly like environment variables on your computer. They're variables that you can access in your code like any other, but since they're not declared or otherwise modified in the source code, they're invisible and can't be accidentally leaked (say when you upload your code to a public git repository). 

- After you create a Lambda function in the AWS portal, you will be greeted with the function's page. Here you can edit the code directly and modify the function's settings. Right below the code window, you will see a menu item for **Environment Variables**. Click Edit to modify them. The **key** is the variable name, and the **value** is the variable's value. Click Save to save these variables in the function. Now you can access the environment variables in the Lambda function's code by calling it using the key. See [AWS Documentation on Environment Variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) for more info.

#### Set up aliases for Lambda functions
- Lambda has a feature called **Aliases**. Aliases are used in combination with function versions to manage what version of the Lambda function will be called by other AWS services. In CrowFacts, aliases are used to connect API Gateway endpoints to the publish and subscribe Lambda functions. 

- In order to create an alias, you must first have a **published function version** to connect it to. Under the Actions menu in the function, click on **Publish new version**, enter a description, and publish it to create a new version. This new version will retain the functionality that it has at the time that it is published. Refer to the [AWS Docs on Lambda Versions](https://docs.aws.amazon.com/lambda/latest/dg/configuration-versions.html) for more detail on what you can do with versioning.

- **Aliases allow you to refer to different versions of a function with the same name.** So for example in API Gateway you can link a GET request with a Lambda function using the alias "API_GET_Version". Then, no matter what function version you connect to that alias, be it 1, 10, or 1000, you will not need to change the API Gateway configuration when you update the Lambda code. You do however, need to modify which function version the alias will point to each time you publish a new version. You *can* alias to the `$LATEST` version in order to have Lambda automatically use whatever code exists when you Save the function, but this is not recommended. You should only alias to function versions that you have tested and verified to work. Further documentation on function aliases is available [here](https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html).

### DynamoDB

More information coming soon!

## Gotchas & Lessons Learned

More information coming soon!

