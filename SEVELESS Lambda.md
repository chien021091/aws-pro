## Lambda 

By default, lambda provides your account with a total concurrency limit of 1 000 concurrent executions across all functions in an AWS region.

- Reserved concurrency: to reserve a portion of your account's for a function. This is useful if you don't want other functions taking up all the available unreserved concurrency.
- Provisioned concurrency : to pre-initialize a number of environment instances for a function. this is useful for reducing cold start latencies.
------------

Reporting batch item failures for lambda functions with an SQS trigger
- By default, when a SQS trigger lambda, lambda executes the functions, if it returns 200, the message deletes automatic from SQS.

In the failure case, if we want to report the batch item failures in the response, signaling to lambda to retry those message later.

```
// Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
// SPDX-License-Identifier: Apache-2.0
import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;
import com.amazonaws.services.lambda.runtime.events.SQSEvent;
import com.amazonaws.services.lambda.runtime.events.SQSBatchResponse;
 
import java.util.ArrayList;
import java.util.List;
 
public class ProcessSQSMessageBatch implements RequestHandler<SQSEvent, SQSBatchResponse> {
    @Override
    public SQSBatchResponse handleRequest(SQSEvent sqsEvent, Context context) {
 
         List<SQSBatchResponse.BatchItemFailure> batchItemFailures = new ArrayList<SQSBatchResponse.BatchItemFailure>();
         String messageId = "";
         for (SQSEvent.SQSMessage message : sqsEvent.getRecords()) {
             try {
                 //process your message
             } catch (Exception e) {
                 //Add failed message identifier to the batchItemFailures list
                 batchItemFailures.add(new SQSBatchResponse.BatchItemFailure(message.getMessageId()));
             }
         }
         return new SQSBatchResponse(batchItemFailures);
     }
}
```
-----
Lambda version:
- We can create any version for lambda and use these version for trigger
- We can create an alias for each version. with alias different, we have the ARN different.