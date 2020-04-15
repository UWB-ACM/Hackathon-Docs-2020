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

- S3 bucket website host
    - Website designed to give users access to facts, or to make their own
- API gateway calls to Lambda
    - Means by which GET or PUT calls are made
- Tables accessed via Lambda function
- Success of call is logged via AWS Cloudwatch
- DynamoDB table returns information on crows
- Information returned via API Gateway call
- Website uses image link and urls to present information on crows

[Visit the website here.](https://crowfacts.uwbhacks.com/)

[View the example Lambda PUT call here.](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_put_user_fact/lambda_function.py)

[View the example Lambda GET call here.](https://github.com/UWB-ACM/crowfacts/blob/master/lambda_get_user_facts/lambda_function.py)


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

