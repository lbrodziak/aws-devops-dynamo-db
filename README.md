<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Fetch Data with AWS Lambda

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-lambda)

**Author:** Łukasz Brodziak  
**Email:** lukasz.brodziak@gmail.com

---

## Fetch Data with AWS Lambda

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-lambda_p9thryj2)

---

## Introducing Today's Project!

In this project, I will demonstrate how to create a DynamoDB table along with an example item and how to retrieve the data using the Lambda function

### Tools and concepts

Services I used were Lambd functions and Dynamo DB. Key concepts I learnt include Lambda functions, Dynamo DB table and item creation, modyfing permissions to Dynamo DB, creating custom IAM policies.

### Project reflection

This project took me approximately 40 The most challenging part was troubleshooting test errors. It was most rewarding to see Lambda finally fetch the data from DB.

I chose to do this project today because I needed it for the three-tier-architecture project. Als I wanted to learn how to work with, simple Dynamo DB.

---

## Project Setup

To set up my project, I created a database using DynamoDB. The partition key is userId.  It is used by DynamoDB to spread out your data across different servers for quick access and efficient querying.

In my DynamoDB table, I added a new item with some test user data. DynamoDB is schemaless, meaning you can add attributes as you need, and every item in your database can have a different set of attributes. This flexibility is one of the key benefits of using a NoSQL database like DynamoDB.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-lambda_a112c3d5)

### AWS Lambda

A Lambda function is a piece of code that you run in AWS Lambda.

You write Lambda functions to do things like process data, respond to events, or run automated tasks.

---

## AWS Lambda Function

My Lambda function has an execution role, which  makes sure the function only has the permissions it needs to do its job, minimizing the potential for security vulnerabilities.

The first half of the code uses the AWS SDK for JavaScript to interact with DynamoDB.

It takes a userId as input, grabs the corresponding data from the UserData table, and returns it to you.

The second half of the code (i.e. beginning with try {) handles potential errors during the database operation, so you get a tailored error message that tells you what went wrong.

The AWS Software Development Kit (SDK) is a set of tools that let developers build apps that interact with AWS.

Think of it like a library of pre-written code that lets you use AWS services without having to write all the code yourself. It gives you functions and classes that simplify common tasks, like creating an S3 bucket or launching an EC2 instance.

There's a different SDK for different programming languages or platforms, and JavaScript is one of them! Your code uses the AWS SDK to use pre-written functions for communicating with DynamoDB and getting data from a table. Without the SDK, you'd have to manually write the code to interact with AWS, which would be much more complex and error-prone.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-lambda_a1b2c3d5)

---

## Function Testing

To test whether my Lambda function works, I run the test from the Test tab in Lambda function details. The test is written in JSON format. If the test is successful, I'd see the data retrieved from the Dynamo DB.

The test displayed a 'success' because the function itself ran but the function's response was actually and access denied error because Dynamo DB is currnetly blocking Lambda function to read its data.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-lambda_u1v2w3x4)

---

## Function Permissions

To resolve the AccessDenied error I needed to attach a Dynamo DB policy to the Lambda eecution role

There were six DynamoDB permission policies I could choose from, but I didn't pick AWSLambdaDynamoDBExecutionRole and  AWSLambdaInvocation-DynamoDB because they did not have GetItem permission in them.

AmazonDynamoDBReadOnlyAccess was the right choice because this one contains GetItem. And since we don't need write permissions then according to Least privilage principle the read only policy was the right choice.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-lambda_3ethryj2)

---

## Final Testing and Reflection

To validate my new permission settings, I rerun the testso on the Lambda funtion. The results were successful  because now Dynamo DB allows my function to retrieve data from it.

Web apps are a popular use case of using Lambda and DynamoDB. For example, I could use lambda function to quickly retrieve data and present it to the user.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-lambda_p9thryj2)

---

## Enahancing Security

While AmazonDynamoDBReadOnlyAccess grants read-only access to all your DynamoDB tables, an inline policy allows for more granular control. In this case, we only want our Lambda function to access the UserData table, so an inline policy is more secure.
It's just like the reason why we gave our Lambda function ReadOnlyAccess instead of full access earlier in this project. We're now narrowing permissions even more - let's take out the unnecessary permissions that ReadOnlyAccess is still giving us.

To create the permission policy, I used visual because it was faster to make.

When updating a Lambda funciton's permission policies, you could risk that the function will top working. I validated that my Lambda function still works by rerunningthe test after updating the permissions.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-lambda_1qthryj2)

---

---
