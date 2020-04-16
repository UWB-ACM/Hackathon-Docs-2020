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
To create a functional user website, which acheived the above goals, we used AWS services to host the website and its services on the cloud. The websites design is described below.

- An [S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) was used as the website host, and was configured with a custom domain, as well as [SSL certification for HTTPS traffic](https://www.freecodecamp.org/news/simple-site-hosting-with-amazon-s3-and-https-5e78017f482a/).
- This website was designed to give users access to facts, or to make their own.
- These user actions are made via [API gateway calls to Lambda functions](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-getting-started-with-rest-apis.html), which execute GET or PUT calls.
- Functions calls create a link to either the CrowFacts or FunFacts [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) tables.
    - **CrowFacts**: NoSQL database that returns species information on crows, found by combination of partition and sort key.
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
- If successful, the information will return succesful to API Gateway, which will return the information retrieved.
- Based on if user made a call to retrieve crow information, or fun facts, the S3 bucket will create a visual representation of the information
    - Website uses the image url's in the "image" column to show crow photos. 
    - Additionally, the website uses the "description" column to give a verbal description of the crows.

[Visit the website here.](https://crowfacts.uwbhacks.com/)

[View the example Lambda PUT call here.](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_put_user_fact/lambda_function.py)

[View the example Lambda GET call here.](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_get_user_facts/lambda_function.py)


The diagram of the system flow is below:
![Visual representation of the archtectural design of crow facts](https://i.postimg.cc/cJmdCg0t/actual-arch.png)

## Implementation Notes

### S3 & Website Construction

More information coming soon!

### API Gateway

More information coming soon!

### Lambda

More information coming soon!

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

More information coming soon!

- Data Pipeline cost
- Reserved words
- More work than initially thought
