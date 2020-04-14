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
2. Create two resources: `getCrowFacts` and `UserFacts`.
3. Ensure that the Lambda functions we wanted to link HTTP methods to 
   were in the AWS account, because the functions have to be available 
   at the time of method creation.
4. Under `getCrowFacts`, create a `GET` method and link it to the 
   corresponding [Lambda function](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_species/lambda_function.py) 
   we wrote for this purpose.
5. Uner `UserFacts`, create a `GET` method and link it to the corresponding 
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

More information coming soon!

### DynamoDB

More information coming soon!

## Gotchas & Lessons Learned

More information coming soon!

