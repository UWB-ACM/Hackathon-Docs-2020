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

When considering importing and exporting database information, there are a number of approaches a developer can take. Two common approaches involving creating your own function, using Lambda, or any other scripting service, or using a pipeline. This section covers some of the different use cases for those options in AWS, as well as provides some sample code for exporting/importing by hand.

### Pipeline
When considering how to import data sets, AWS's Data Pipeline service is a service that eleminates many of the manual steps a developer would have to take. It is a process that is flexible to the dataset, scalable, and easily processed in parallel - which means it can process a lot of data very quickly.

As a turnkey solution, Data Pipeline works with most datasets, and can be configured through a drag-and drop interface. However, its flexibility comes at a cost, even when your import or export fails. For larger projects, such as company databases, where information needs to be consistantly moving, this is a great solution. However, for smaller projects, it can easily eat through the budget given for the Hackathon, despite it's effectiveness. As such, smaller projects are safer with building their own import/export functions with Lambda.

### Lambda
This option takes a bit of programming, and can be frustrating to implement, as it is not as flexible as Data Pipeline can be with converting information. However, it's a heckuva lot cheaper for small projects, and the user is in full control over what they are doing.

Lambda is a scripting service on AWS which works based on triggers, code, and output destination. For importing/exporting data, you would need to build your own Lambda function to handle for cases. It's not flexible like pipeline, and you will need to handle the parsing logic yourself.

However - this solution works great for smaller projects. Learning how to set triggers for importing data, and writing code for exporting your data, allows for the developer to have full control over what AWS is doing with their data, and allows you to avoid any hidden fees. It can serve your exact purpose, and can be changed as needed. Generally, for independant projects on a budget, making your own Lambda function would be the best choice. It's also a lot of fun!

#### Import using DynamoDB

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

.CSV (by hand)
```
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
```

.JSON (by hand)
```
DynamoDB does not have inbuilt exporting of .JSON files, but it is not to difficult to get your information exported with using Data Pipeline, or Lambda. It is a bit of a brute force solution, but it is simple and functional.

To do so, follow the steps listed above to export a .CSV file. Then, convert that .CSV file into .JSON. It's that easy!

There are a number of sites that can do the conversion, but an example of one such sight can be found here: https://csvjson.com/csv2json
```

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

When working with tables in AWS, it's important to keep mind of a few words that should not be used. These are words in use by the database itself for a variety of different reasons, and using these terms as a primary key, or sort key, will cause errors when trying to call those columns from a JavaScript call.

A few example words are:
- LOCATION
- ATTRIBUTE
- BLOB
- CLASS
- DIAGNOSTICS
- GROUP

**DO NOT USE THESE WORDS**

They will only cause problems for you down the line!

Before finalizing your database schema, be sure to check ALL TERMS against these reserved words. If you use on of these terms for your partition key, or sort key, there will be no way to change them. Failure to double check may mean your group will have to terminate your table, and recreate it so it no longer uses these terms.

For a full list of terms, check out the resource for DynamoDB: [Reserved Words in DynamoDB
PDF](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ReservedWords.html)

Each AWS database service will have its own set of reserved words, so be sure to check those out early, and reference them often before finalizing your tables.
