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
- Website designed to give users access to facts, or to make their own.
- These user actions as made via [API gateway calls to Lambda functions](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-getting-started-with-rest-apis.html), which execute GET or PUT calls.
- Functions calls create link to either the CrowFacts or FunFacts [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) tables.
    - **CrowFacts**: NoSQL database that returns species information on crows, found by combination of partition and sort key.
        - Schema
            - Primary key: "CrowSpecies" + "habitat"
            - Sort: 
                - "description": a short decription of the crows appearance
                - "SubSpecies": specifies what subspecies a crow is, e.g., Florida Crow 
                - "scientific": the scientific name of the crow species 
                - "image": url to an image of the crow species
    - **FunFacts**: A mixture of actual fun facts about crows, and user inputted facts, sanitized at time of Lambda call
        - Schema
            - Primary key: "fact"
            - Sort: 
                - "source": the source of who inputted the fact
- Success of call GET or PUT call is logged via [AWS Cloudwatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html).
- If successful, the information will return succesful to API Gateway, which will return the information retrieved.
- Based on if user made a call to retrieve crow information, or fun facts, the S3 bucket with create a visual representation of the information
    - Website uses the image url's in the "image" column to show crow photos. 
    - Additionally, uses the "description" column to give a verbal description of the crows.

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
The information presented on the website was stored on a DynamoDB database table. This is a NoSQL database, and there are important factors to consider when implementing this in a project

#### Setting up Permissions
Interaction with other AWS services requires the set up of other Amazon services. IAM permissions. Etc...

#### Designing table schema
NoSQL is very flexible, and allows for quick table's to be set up. This is dangerous, however, and can become a crutch for lazy table design. Consider carefully what your project is trying to require, and design a schema with the intention of not making changes to it. Although it is tempting to add and remove columns as needed, this quickly becomes untentable. Etc...

#### Settings?
todo

#### Section on other database related things
todo

## Gotchas & Lessons Learned

More information coming soon!

- Data Pipeline cost
- Reserved words
- More work than initially thought

