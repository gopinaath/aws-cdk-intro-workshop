+++
title = "CloudWatch Logs"
weight = 500
+++

## Viewing CloudWatch logs for our Lambda function

The first thing to do is to go and look at the logs of our hit counter AWS
Lambda function.

There are many tools that help you do that like [SAM
CLI](https://github.com/awslabs/aws-sam-cli) and
[awslogs](https://github.com/jorgebastida/awslogs). In this workshop, we'll show
you how to find your logs through the AWS console.

1. Open the [AWS Lambda console](https://console.aws.amazon.com/lambda/home) (make sure you
   are connected to the correct region).

2. Click on the __HitCounter__ Lambda function
   (the name should contain the string `CdkWorkshopStack-HelloHitCounter`):
    ![](./logs1.png)

3. Click on __Monitoring__
    ![](./logs2.png)

4. Click on __View Logs in CloudWatch__. This will open the AWS CloudWatch console.
    ![](./logs3.png)

5. Select the most-recent log group.

6. Look for the most-recent message containing the string "errorMessage". You'll likely see something like this:


   ```json
   {
       "errorType": "AccessDeniedException",
       "errorMessage": "User: arn:aws:sts::XXXXXXXXX:assumed-role/CdkWorkshopStack-HelloHitCounterHitCounterHandlerS-TU5M09L1UBID/CdkWorkshopStack-HelloHitCounterHitCounterHandlerD-144HVUNEWRWEO is not authorized to perform: dynamodb:UpdateItem on resource: arn:aws:dynamodb:us-east-1:XXXXXXXXX:table/CdkWorkshopStack-HelloHitCounterHits7AAEBF80-1DZVT3W84LJKB",
       "stack": [
           "AccessDeniedException: User: arn:aws:sts::XXXXXXXXX:assumed-role/CdkWorkshopStack-HelloHitCounterHitCounterHandlerS-TU5M09L1UBID/CdkWorkshopStack-HelloHitCounterHitCounterHandlerD-144HVUNEWRWEO is not authorized to perform: dynamodb:UpdateItem on resource: arn:aws:dynamodb:us-east-1:XXXXXXXXX:table/CdkWorkshopStack-HelloHitCounterHits7AAEBF80-1DZVT3W84LJKB",
           "at Request.extractError (/var/runtime/node_modules/aws-sdk/lib/protocol/json.js:48:27)",
           "at Request.callListeners (/var/runtime/node_modules/aws-sdk/lib/sequential_executor.js:105:20)",
           "at Request.emit (/var/runtime/node_modules/aws-sdk/lib/sequential_executor.js:77:10)",
           "at Request.emit (/var/runtime/node_modules/aws-sdk/lib/request.js:683:14)",
           "at Request.transition (/var/runtime/node_modules/aws-sdk/lib/request.js:22:10)",
           "at AcceptorStateMachine.runTo (/var/runtime/node_modules/aws-sdk/lib/state_machine.js:14:12)",
           "at /var/runtime/node_modules/aws-sdk/lib/state_machine.js:26:10",
           "at Request.<anonymous> (/var/runtime/node_modules/aws-sdk/lib/request.js:38:9)",
           "at Request.<anonymous> (/var/runtime/node_modules/aws-sdk/lib/request.js:685:12)",
           "at Request.callListeners (/var/runtime/node_modules/aws-sdk/lib/sequential_executor.js:115:18)"
       ]
   }
   ```

---

It seems like our Lambda function can't write to our DynamoDB table. This
actually makes sense - we didn't grant it those permissions! Let's go do that
now.

