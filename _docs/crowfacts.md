---
layout: default
category: Projects
title: CrowFacts
order: 1
permalink: /crowfacts.html
---

# CrowFacts, an Example Project for AWS
This example project uses the following services:
- DyanmoDB
- S3
- API Gateway
- Lambda
- IAM

# Connecting the Front-End to the Back-End
Services used: IAM, Lambda, API Gateway

## Step 1: Setting up IAM permissions

1. Go to the [IAM services](https://console.aws.amazon.com/iam/home?region=us-west-2#/home) page and select "roles" on the left menu, and click on the blue "Create role" button.

2. Select the "Lambda" use case and click Next.

3. Here we will allow our Lambda functions to access all of our DynamoDB instances and do whatever they want. This is extremely bad practice and allows hackers and bugs to flourish. But for this example, we won't worry about setting up specific restrictions.
In the search bar, search: "DynamoDBFullAccess" and select the checkbox beside the option that says "AmazonDynamoDBFullAccess". Click Next.

4. Skip the "Add tags" section by clicking Next. Give your role a meaningful name, in this case we will call it "DynamoDB-God". Then click on "Create role".


## Step 2: Create a lambda to connect to your backend

1. Head over to the [lambda](
https://us-west-2.console.aws.amazon.com/lambda/home?region=us-west-2#/functions) service on AWS and click on the orange "Create Function" button.

2. Select the "Author from scratch box". Set the function name to something memorable, in this case we will do "GetSingleCrow". Select the Runtime to a language you're comfortable in, for this example we will use Python 3.8 because it's easier to use than Node.js and faster.

3. Click the little gray arrow that says "Choose or create an execution role" and select "use an existing role" and select the IAM role we created in the previous step, in our case: DynamoDB-God. Click the next button.

## Step 3: Make your Lambda useful (with code!)

1. Here is the code we use for retrieving a single item from the database. Please take the time to read the comments in the code as they explain what the code does.

**HINT:** Most of the code can be reused to accomodate for PUT, POST, GET, etc. calls.

**HINT 2:** Make sure you are not using reserved keywords in your code! [These](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ReservedWords.html) are all the reserved keywords for DynamoDB.


```
import json
import boto3

def lambda_handler(event, context):
    
	# Check that the incoming request has the necessary parameters. In this case we are expecting 'CrowSpecies' and 'location'. 
	# For a GET method, the incoming parameters need to be the primary/sorting keys of the database.
	# For a POST method, the incoming parameters need to be the new data you want to add to the database
    
	try:
        species = event['CrowSpecies']
        loc = event['location']
    except Exception as e:
		return e.response['Error']['Message']
    
	# Create a connection to the DynamoDB instance of AWS
    
	dynamodb = boto3.resource('dynamodb')
    
	# Create a connection to the specific table we want from our DynamoDB. In this case, 'CrowFacts'.
    
	table = dynamodb.Table('CrowFacts')

    # Make a request to our table to retrieve the item given the parameters.
	# For POST or PUT change 'table.get_item' to 'table.post_item' and change 'Key' to 'Item'
	
	try:
		response = table.get_item(
			
			# The Key is what we use to query the database (ie: We need the key to know what item to return)
			
			Key={
				'CrowSpecies': species,
				'location': loc
			}
		)
	except Exception as e:
		return e.response['Error']['Message']
	else:
		
		# We return a JSON object with status code 200 meaning success. And we put the response (aka result from the database) in the body.

		return {
			'statusCode': 200,
			'body': response
		}
        
```

2. Let's test the code! 
**You should have at least 1 single item in your database, if you don't, go back to DynamoDB and add an item then continue.**

Press the orange "save" button in the top right, you need to do this whenever you change any code in the Lamnda.
Then click on the white "Test" button, select "create new test event", don't touch the "Event Template". Set the "event name" to something meaningful, in this case we use "GetCrowTest1".

Setup your test code with the correct expected and actual parameters. In this example, we want to get the item that has the following values:
`CrowSpecies = American crow`
`location = Florida`
So we set up our JSON request like this: 

**Hint 1: Don't forget the open and closing {}**
**Hint 2: Don't put a comma after the last line!**
```
{
  "CrowSpecies": "American crow",
  "location": "Florida"
}
```

Click on the orange "Create" button in the bottom right.

Now for the moment of truth... In the dropdown beside the "Test" button select the test we just created then click the "Test" button.
Under the Test button a Red/Green banner should display, and you can click on the "Details" dropdown to display the test results.

**Hint: If you are using Python you may get an error saying "inconsistent use of tabs and spaces...". In this case just go back to your code and replace all the indentations with tabs.**

If you see the item you are expecting in the "Execution result" banner at the top, then you've successfully written your first lambda function!!

## Step 4: Connecting Lambda to API Gateway

1. In order to connect our Lambda to API Gateway we need to create an Alias for our Lambda. Open up your Lambda function and select "Actions" at the top and then select "Create alias".
Give you alias an easy to spell name, in this case we will use "Latest". Ignore the description. Under "Version" make sure to select "$LATEST". Ignore the "Additional version".

2. Head over to [API Gateway service](https://us-west-2.console.aws.amazon.com/apigateway/main/apis?region=us-west-2).
Click on the orange "Create API" button in the top right.
Under the REST API box, select "Build".
Select "REST", and "New API".
Give your API a meaningful name, in this case we use "Crows".
Ignore the description and Endpoint Type options.

3. Near the top of the screen should be an "Actions" dropdown. Select that, and select "Create Method".
On the dropdown that just appeared, select "GET" and click the checkbox beside it.

4. A section on the screen should have opened up with the header similar to "/ - GET - Setup".
Select "Lambda Function".
Do not select "Use Lambda Proxy integration" (unless you know what you're doing).
Leave the "Lambda Region" untouched.
In the "Lambda Function" box, start typing the name of the Lambda we created earlier. Select the Lambda once you see it pop up.
**After the name of the Lambda, add a colon and type the name of the alias you created.**
So in our case:
Lambda: GetSingleCrow
Alias: Latest
So I'd type: GetSingleCrow:Latest in the "Lambda Function" text box.
Leave the "Use default timeout" option selected.
Click Save, then click Ok and wait for a few moments.

5. Now we need to launch the API so that the outside world can access it.
Under the "Actions" dropdown near the top, select "Deploy API".
In "Deployement stage" select  "[New Stage]".
Give your stage a name, in this case we called it "test".
Hit the "Deploy" button.
Congrats! Now your API is live!

# ToDo:
## Connecting the API to the front-end
## What is CORS and how to use it