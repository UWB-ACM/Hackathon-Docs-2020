---
layout: default
category: Platforms - AWS
title: Resources
order: 3
permalink: /aws_resources.html
---

# AWS Resources, SDK Links, Tutorials, and Helpful Tidbits

## Importing Large Datasets

Pipieline vs Lambda functions

### Pipeline
Costs money!!! So much money!!! Please do not use this!!!!!!!!!!!!!!!

### Lambda
Using S3 and DynamoDB

#### Import

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

.CSV
```python
# ? TODO ?
```


#### Export

.JSON (by hand)
```python
# BRUTE FORCE SOLUTION
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

.CSV (by hand)
```python
# BRUTE FORCE SOLUTION
```

.CSV (Lambda)
```python
# ? TODO ?
```

