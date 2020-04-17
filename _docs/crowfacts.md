---
layout: default
category: Projects
title: CrowFacts
order: 1
permalink: /crowfacts.html
---

# CrowFacts, an Example Project for AWS

One of the biggest problems faced by humanity is that we have no way to easily get facts about crows. Until now.

## Goals

- Users can get information about crows
- Users can insert new information into the database, and get data from other users
- Host all data on AWS S3 and using DynamoDB to access and create new entries

## User Experience

1. The user goes to our website and has the option to get crow facts or enter new crow facts.

2. The user can input their own crowfacts (assuming it doesn't contain any no-no words). A list of all submitted words will show on the right side.

<iframe src="https://giphy.com/embed/W4PtD360Ev9x4myZH0" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/run-mjkahn-crow-W4PtD360Ev9x4myZH0">via GIPHY</a></p>

## Architecture
To create a functional user website which achieved the above goals, we used AWS services to host the website. The website's design is described in detail below.

- An [S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) was used as the website host, and was configured with a custom domain as well as [SSL certification for HTTPS traffic](https://www.freecodecamp.org/news/simple-site-hosting-with-amazon-s3-and-https-5e78017f482a/).
- This website was designed to give users access to facts, or to make their own.
- These user actions are made via [API gateway calls to Lambda functions](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-getting-started-with-rest-apis.html), which execute GET or PUT calls.
- Function calls create a link to either the CrowFacts or FunFacts [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) tables.
    - **CrowFacts**: NoSQL database that returns species information on crows. Facts are retrieved by combination of partition and sort key.
        - Primary key: "CrowSpecies" + "habitat"
        - Sort: 
            - "description": a short decription of the crows appearance
            - "SubSpecies": specifies what subspecies a crow is, e.g. Florida Crow 
            - "scientific": the scientific name of the crow species 
            - "image": url to an image of the crow species
    - **FunFacts**: A mixture of actual fun facts about crows, and user inputted facts, sanitized at time of Lambda call.
        - Primary key: "fact"
        - Sort: 
            - "source": the source of who inputted the fact
- Success of GET or PUT call is logged via [AWS Cloudwatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html).
- If successful, the information will be returned to API Gateway which will in turn return the information retrieved to the website.
- Based on if user made a call to retrieve crow information, or fun facts, the S3 bucket will create a visual representation of the information
    - Website uses the image url's in the "image" column to show crow photos. 
    - Additionally, the website uses the "description" column to give a verbal description of the crows.

[Visit the website here.](https://crowfacts.uwbhacks.com/)

[View the Lambda code for the PUT call here.](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_put_user_fact/lambda_function.py)

[View the Lambda code for the GET call here.](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_get_user_facts/lambda_function.py)


The diagram of the system flow is below:
![Visual representation of the archtectural design of crow facts](https://github.com/UWB-ACM/Hackathon-Docs-2020/blob/erica/crowfactsDocs/assets/imgs/actual_arch.png?raw=true)

## Implementation Notes

### S3 & Website Construction

#### Website Creation

The website was written on a local machine and deployed to S3.

The website for this project was built with a straightforward HTML, 
CSS, and vanilla JavaScript stack. jQuery was used to perform HTTP 
requests to the database call pipeline. The complete code for the 
website is available [in the project repository](https://github.com/UWB-ACM/crowfacts/tree/master/website).

To streamline site deployment, we used a [GitHub Action](https://github.com/marketplace/actions/s3-sync) 
which will copy specified files to the S3 bucket when changes are 
pushed to master. This was great, because we didn't have to manually 
update the contents of the S3 bucket during website development.

#### Domain Configuration

To set up the custom domain for the site, we did the following:

1. Chose a desired URL for the site: https://crowfacts.uwbhacks.com
2. Created an S3 bucket named as the full URL and set it up for static website 
   serving, by following [these instructions from the AWS docs](https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html).
   Because we already owned the domain, we skipped all steps related to 
   AWS Route 53 in that tutorial.
3. In Cloudflare, our DNS management service, we created the `crowfacts` 
   subdomain for `uwbhacks.com` as a `CNAME` record.
4. We then updated the bucket accesss policies [with this template 
  provided by Cloudflare](https://support.cloudflare.com/hc/en-us/articles/360037983412-Configuring-an-Amazon-Web-Services-static-site-to-use-Cloudflare#77nNxWyQf69T1a78gPlCi9).

We used the following tutorials and resources as references:

* [AWS Docs: Hosting a Static Website on S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html)
* [AWS Docs: Hosting an S3 Site with a Custom Domain](https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html)
* [Cloudflare docs: Configuring S3 Static Site to Use Cloudflare](https://support.cloudflare.com/hc/en-us/articles/360037983412-Configuring-an-Amazon-Web-Services-static-site-to-use-Cloudflare)

### API Gateway

#### Basic Configuration

The basic API Gateway configuration we did was:

1. Create a new API in the AWS Console. We named ours `crowfacts` and 
   chose the `REST` API protocol.
2. Create two resources: `getCrowSpecies` and `UserFacts`.
3. Ensure that the Lambda functions we wanted to link HTTP methods to 
   were in the AWS account, because the functions have to be available 
   at the time of method creation.
4. Under `getCrowSpecies`, create a `GET` method and link it to the 
   corresponding [Lambda function](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_species/lambda_function.py) 
   we wrote for this purpose.
5. Under `UserFacts`, create a `GET` method and link it to the corresponding 
   [Lambda function](https://github.com/UWB-ACM/crowfacts/tree/master/lambda_get_user_facts)
   we wrote for this purpose.
6. Under `UserFacts`, create a `POST` method and link it to the corresponding 
   [Lambda function](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_put_user_fact/lambda_function.py) 
   we wrote for this purpose.
7. Under the "Actions" menu dropdown, click "Enable CORS". Accept all default 
   settings and click the "Enable" button.
8. Under the "Actions" menu dropdown, click "Deploy API". Create a new 
   deployment stage and click Deploy.
9. After deployment, click on "Stages" in the left-hand menu for the API. 
   Expand the resource tree and click on each method to get the URL endpoint 
   for that method & resource.

Ta-da! After completing these steps, we were able to successfully `cURL` the 
endpoints for our API and validate that it worked as expected.

#### Custom HTTP Error Handling

This application processes user input, and as such, we wanted to validate 
that input and return errors to the client when the input didn't meet our 
specifications for database entries. We did this by setting up custom error 
handling in the API Gateway `POST` method under the `/UserFacts` resource.

To enable the client to receive custom HTTP errors based on the Lambda 
function's responses, we set up Integration and Method responses.
We used [this AWS blog post](https://aws.amazon.com/blogs/compute/error-handling-patterns-in-amazon-api-gateway-and-aws-lambda/) 
as our primary reference.

##### Lambda Function Configuration

Instead of returning a JSON blob from Lambda, we had to raise an exception 
in the Lambda function to get API Gateway to use custom error responses.
An example of the correct syntax is:

```python3
import json
import boto3
import os

def lambda_handler(event, context):
    raise Exception('This is an example exception from Lambda.')
```

The actual implementation of where we raised exceptions [can be found 
here](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_put_user_fact/lambda_function.py#L5).

##### Method Responses from API Gateway to the Client

In order to set up error handling from Lambda, we had to first define 
what [status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) 
we wanted to return to the client. Each status code indicates a different 
type of error and is accompanied by a different error message which 
the client site can parse.

We used the following error codes:

* `400: Bad Request`
  This error will be returned when the client did not send the required 
  JSON keypairs the DynamoDB table needed.
* `403: Forbidden`
  This error will be returned when the client's request had valid JSON 
  keys, but the values contained verbiage which is not appropriate for all 
  ages.
* `500: Internal Server Error`
  This error will be returned when the Lambda function encountered an 
  error trying to insert the data into the DynamoDB table.

For each of these method responses, we had to add the `Access-Control-Allow-Origin` 
header to the status code so that the custom error would fulfill the 
browser's CORS requirement. Clicking "Enable CORS" in the console action 
dropdown only enables CORS for the standard `200` status code response.

After completing this step, our method response section looked like this:

![Method Response Configuration for Custom Errors to Client](/assets/custom_error_method_headers.png)

##### Integration Responses from Lambda to API Gateway

After we designated the HTTP error status codes we wanted to use in 
Method Response, we had to define what Lambda errors mapped to what 
status codes.

To do this, we created integration responses for each custom method. 
For each integration response, we provide a [regular 
expression](https://en.wikipedia.org/wiki/Regular_expression) which 
searches the error message returned from Lambda. If a match for the 
regular expression is found, API Gateway will return the HTTP error code 
associated with that regular expression.

We have the option to define post-processing for the Lambda error message, 
but we elected to return Lambda's raw string; this is called a _passthrough 
response_.

Our definitions look something like this:

![Configuration for integration responses in API Gateway](/assets/integ_response_setup_with_regex.png)

After we created the Integration responses for our custom errors, we had 
to enable CORS for the errors by hand. We did this by creating a new 
header mapping for each Lambda Error Regex entry. We set the header to 
`Access-Control-Allow-Origin` and the mapped value to `'*'`. The syntax 
for this step is very important; it must respect the CORS protocol.

After setting this up, our integration responses looked like this:

![Integration Response Configuration for Custom Errors from Lambda](/assets/custom_error_integration_headers.png)

##### Handling Error Responses in the Client

The jQuery function used by the client to makes the request requires a 
definition of how to handle HTTP errors. We chose to simply display the 
error message from API Gateway. The definition for how we handled 
the custom errors can be seen 
[here](https://github.com/UWB-ACM/crowfacts/blob/master/website/scripts/dynamo.js#L74).

### Lambda

AWS Lambda handles all of CrowFacts' computation. Within AWS Lambda, there are several important things that must be configured in order for CrowFacts' code to operate correctly.

#### Give Lambda access to DynamoDB
By default, creating a Lambda function from scratch, adding DynamoDB code, and pressing "Test" won't work. Why? Simple, because the Lambda function does not have *access* to DynamoDB. Cloud systems operate on a least-required permissions model, meaning that in order for your Lambda function to access DynamoDB, you must explicitly grant Lambda access to DynamoDB (and any other AWS services you might want Lambda to access). Refer to [AWS documentation on IAM permissions for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/authentication-and-access-control.html), and check out [the TL;DR explanation of IAM on the documentation site for this hackathon]({% link _docs/aws_secrets.md %}).
    
#### Set up environment variables for Lambda functions
- In order to keep the code looking clean, and to avoid writing publicly available code that contains sensitive information (including naughty words), we can use Environment Variables. These are exactly like environment variables on your computer. They're variables that you can access in your code like any other, but since they're not declared or otherwise modified in the source code, they're invisible and can't be accidentally leaked (say when you upload your code to a public git repository).

- After you create a Lambda function in the AWS portal, you will be greeted with the function's page. Here you can edit the code directly and modify the function's settings. Right below the code window, you will see a menu item for **Environment Variables**. Click Edit to modify them. The **key** is the variable name, and the **value** is the variable's value. Click Save to save these variables in the function. Now you can access the environment variables in the Lambda function's code by calling it using the key. See [AWS Documentation on Environment Variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) for more info.

#### Set up aliases for Lambda functions
- Lambda has a feature called **Aliases**. Aliases are used in combination with function versions to manage what version of the Lambda function will be called by other AWS services. In CrowFacts, aliases are used to connect API Gateway endpoints to the publish and subscribe Lambda functions. 

- In order to create an alias, you must first have a **published function version** to connect it to. Under the Actions menu in the function, click on **Publish new version**, enter a description, and publish it to create a new version. This new version will retain the functionality that it has at the time that it is published. Refer to the [AWS Docs on Lambda Versions](https://docs.aws.amazon.com/lambda/latest/dg/configuration-versions.html) for more detail on what you can do with versioning.

- **Aliases allow you to refer to different versions of a function with the same name.** So for example in API Gateway you can link a GET request with a Lambda function using the alias "API_GET_Version". Then, no matter what function version you connect to that alias, be it 1, 10, or 1000, you will not need to change the API Gateway configuration when you update the Lambda code. You do however, need to modify which function version the alias will point to each time you publish a new version. You *can* alias to the `$LATEST` version in order to have Lambda automatically use whatever code exists when you Save the function, but this is not recommended. You should only alias to function versions that you have tested and verified to work. Further documentation on function aliases is available [here](https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html).

### DynamoDB
The information presented on the website was stored in a DynamoDB database table. This is a NoSQL database, and there are important factors to consider when implementing this in a project.

#### Consider your usage
NoSQL databases do not require a defined relational schema as SQL databases do. They are flexible, allowing attributes to be introduced as needed. If the relationships between data points are important, then consider AWS's relational database options such as [Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html).

#### Create a table
Your table is the place where you store your information. Each table requires a [primary key](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.Partitions.html), which can be a single item, or a combination of a partition and sort key. Consider your choice carefully, as there is no way to change your primary key once the table is created. [Reference the list of reserved words for DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ReservedWords.html), to make sure you are not using one of those words, which will cause problems down the line.

#### Set up Permissions
Interaction with other AWS services requires the set up of permissions, so that those services may interact with your database. AWS utilizes [IAM permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html), which can be made as broad or as narrow as your use-case requires. Setting your permissions up is a key factor for being able to use Lambda functions on your database or make API Gateway calls.

#### Import/Export Datasets
There are multiple approaches for handling the import and export of your DynamoDB information. See the following links for some options on how to implement it.

[AWS Resources, SDK Links, Tutorials, and Helpful Tidbits: Importing Large Datasets](https://github.com/UWB-ACM/Hackathon-Docs-2020/blob/master/_docs/aws_resources.md#importing-large-datasets)</br>
This document goes over some options for importing and exporting data in DynamoDB. It includes functional code for Lambda calls to import/export a .JSON file.

[What is AWS Data Pipeline?](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/what-is-datapipeline.html)</br>
This is the AWS documentation on Data Pipeline, a service which can be used to import/export database information. Can become costly quickly, so use with caution.

[Mockaroo](https://mockaroo.com/)</br>
A good website to use to get some mock data to use when testing your import/export methods.

## Gotchas & Lessons Learned

- Data Pipeline cost
- Reserved words
- More work than initially thought
