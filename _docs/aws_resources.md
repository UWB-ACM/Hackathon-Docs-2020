---
layout: default
category: Platforms - AWS
title: Resources
order: 3
permalink: /aws_resources.html
---

# AWS Resources, SDK Links, Tutorials, and Helpful Tidbits

## SDK Links

- [JavaScript](https://aws.amazon.com/sdk-for-browser/)

- [Python](https://aws.amazon.com/sdk-for-python/)

- [PHP](https://aws.amazon.com/sdk-for-php/)

- [.NET](https://aws.amazon.com/sdk-for-net/)

- [Ruby](https://aws.amazon.com/sdk-for-ruby/)

- [Java](https://aws.amazon.com/sdk-for-java/)

- [Go](https://aws.amazon.com/sdk-for-go/)

- [Node.js](https://aws.amazon.com/sdk-for-node-js/)

- [C++](https://aws.amazon.com/sdk-for-cpp/)

### Still looking for more? [Here's some additional tools!](https://aws.amazon.com/getting-started/tools-sdks/)

## Importing Large Datasets

When considering importing and exporting database information, there are a number of approaches a developer can take. Two common approaches involving creating your own function using a service like AWS Lambda, or using a pipeline. This section covers some of the different use cases for those options in AWS, as well as provides some sample code for exporting/importing by hand.

### AWS Data Pipeline
When considering how to import data sets, AWS's Data Pipeline service is a service that eliminates many of the manual steps a developer would have to take. It is a process that is flexible to the dataset, scalable, and easily processed in parallel - which means it can process a lot of data very quickly.

As a turnkey solution, Data Pipeline works with most datasets, and can be configured through a drag-and drop interface. However, its flexibility comes at a cost, even when your import or export fails. For larger projects, such as company databases, where information needs to be consistantly moving, this is a great solution. However, for smaller projects, it can easily eat through the budget given for the Hackathon, despite its effectiveness. As such, smaller projects are safer with building their own import/export functions with Lambda.

### Lambda
This option takes a bit of programming, and can be frustrating to implement, as it is not as flexible as Data Pipeline can be with converting information. However, it is significantly cheaper for small projects, and the user is in full control of each aspect of the process.

Lambda is a serverless computation product on AWS which works based on triggers, code, and output destination. For importing/exporting data, you would need to build your own Lambda function to handle for cases. It's not flexible like Pipeline, and you will need to handle the parsing logic yourself.

However - this solution works great for smaller projects. Learning how to set triggers for importing data, and writing code for exporting your data, allows for the developer to have full control over what AWS is doing with their data, and allows you to avoid any hidden fees. It can serve your exact purpose, and can be changed as needed. Generally, for independant projects on a budget, making your own Lambda function would be the best choice. It's also a lot of fun!

#### Import Into DynamoDB With Lambda

.JSON
```python
import json
import boto3 #sdk
s3_client = boto3.client('s3')
dynamodb = boto3.resource('dynamodb')

# BOTO 3 DOCS: https://boto3.amazonaws.com/v1/documentation/api/latest/index.html
# Python vers 3.6

def lambda_handler(event, context):
    print(str(event))
    #retrieves name of S3
    bucket = event['Records'][0]['s3']['bucket']['name']
    #retrieves name of actual file in S3
    json_file_name= event['Records'][0]['s3']['object']['key']
    #retrieves the json info location in S3
    json_object = s3_client.get_object(Bucket = bucket, Key = json_file_name)
    #reads S3 info
    jsonFileReader = json_object['Body'].read()
    #parses info into 'dict' format
    jsonDict = json.loads(jsonFileReader)
    #grabs the table to import into
    table = dynamodb.Table('FactsImport')
    #goes through each item within the dict
    #necessary for json files with more than one object
    for k in jsonDict
        #places each item within table
        table.put_item(Item = k)
```

#### Export using DynamoDB

##### Export to CSV Using Built-in Functionality

DynamoDB actually has inbuilt exporting for .CSV files!
Just follow these steps:
- Navigate to the DynamdoDB tables tab
- Select the table you want to export from the table's tab
- Currently, you should be on your table's "overview" page
- Next to the "overview" tab, select the "items" tab
- You should currently see your populated table
- Next to your primary/partition key, there should be a checkbox
- Click on this box
- All boxes next to your table items should be selected
- Now, there should be two buttons: "Create Item", and "Actions"
- Select "Actions"
- Under "Actions", there is the option "Export to .CSV"
- Select this option
- A .CSV file will be automatically saved to your computer

This can be done for as many, or as little table items as needed.

##### Export Data to JSON By Hand

DynamoDB does not have inbuilt exporting of .JSON files, but it is not too difficult to export your information using Data Pipeline or Lambda. It is a bit of a brute force solution, but it is simple and functional.

To do so, follow the steps listed above to export a .CSV file. Then, convert that .CSV file into .JSON. It's that easy!

There are a number of sites that can do the conversion, but an example of one such sight can be found here: https://csvjson.com/csv2json

.JSON (Lambda)
```python
import json
import boto3 #sdk
s3_client = boto3.client('s3')
client = boto3.client('dynamodb')
s3 = boto3.resource("s3")

# BOTO 3 DOCS: https://boto3.amazonaws.com/v1/documentation/api/latest/index.html
# Python vers 3.6

def lambda_handler(event, context):
    # gets all info from table you want to export
    response = client.scan(
    TableName='FactsImport',
    )

    # the s3 bucket you want to upload into
    bucket_name = "crobfactsample"
    file_name = "info1.json"
    # used for making your file to upload to s3
    lambda_path = "/tmp/" + file_name
    # used for where your file will be uploaded within s3 bucket
    # stuff before '/' will be path
    # will create new path if none there before
    s3_path = "crowTest/" + file_name
    # handles writing the 'dict' data type to .json file
    # good for keeping formatting
    with open(lambda_path, 'w') as f:
        json.dump(response, f, sort_keys = True)       
    # uploads file from lambda_path to your s3_path, in your s3 bucket
    s3.Bucket(bucket_name).upload_file(lambda_path, s3_path)
```


## Reserved Words

When working with DynamoDB tables in AWS, it's important to keep in mind that there are certain words that are reserved by DynamoDB. These are words in use by the database itself for a variety of different reasons, and using these terms as a primary key, or sort key, will cause errors when trying to call those columns from a JavaScript call.

A few example words are:
- LOCATION
- ATTRIBUTE
- BLOB
- CLASS
- DIAGNOSTICS
- GROUP

**DO NOT USE THESE WORDS**

They will only cause problems for you down the line!

Before finalizing your database schema, be sure to check ALL TERMS against these reserved words. If you use one of these terms for your partition or sort key, there will be no way to change them. Failure to double check may mean your group will have to terminate your table and recreate it so it no longer uses these terms.

For a full list of terms, check out the resource for DynamoDB: [Reserved Words in DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ReservedWords.html)

Each AWS service will have its own set of reserved words, so be sure to check them out prior to finalizing your implementation.


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

[S3 Video Tutorial Available Here](https://www.youtube.com/watch?v=P_B79xXf-w8)

## AWS Lambda & DynamoDB | AWS Serverless Tutorial
This tutorial as uses AWS Lambda, AWS API Gateway and DynamoDB to create a serverless backend for your application. This is a three
part series that can be accessed on "Cloud Path" via youTube.

* [Lambda DynamoDB Video Tutorial Available Here](https://www.youtube.com/watch?v=VGerk8hrP9U)
* [Reserved Words In DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ReservedWords.html)

## AWS Lamda Tutorial | Use AWS CloudWatch Events to Schedule a Lambda Function Invocation
In this AWS Lambda tutorial we set up a basic hello world Lambda function that's triggered by a scheduled CloudWatch event to run every minute. This video is part of the AWS Serverless tutorial series.

[Lambda & CloudWatch Video Tutorial Available Here.](https://www.youtube.com/watch?v=-v4LMV5DAD4&list=PLD_RqipW0-9s-u1HXTglYV8Aam-5P3XLi&index=7&t=0s)

## How to Create EC2 Instance in AWS Step-by-step
How to create EC2 instance in AWS step by step, including creating the EC2 resource and launching the instance.

[EC2 Video Tutorial Available Here](https://www.youtube.com/watch?v=1x5dWRaLvIw)

## AWS EC2 Linux Launch & Connection Using SSH

This video will show how to use a PuTTY private key to connect to your Amazon EC2 Linux instance.

* [EC2 Linux Launch Video Available Here](https://www.youtube.com/watch?v=v0g1M5bb9u4)
