# Amazon SQS short and long polling<a name="sqs-short-and-long-polling"></a>

 Amazon SQS provides short polling and long polling to receive messages from a queue\. By default, queues use short polling\.

With *short polling*, the [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html) request queries only a subset of the servers \(based on a weighted random distribution\) to find messages that are available to include in the response\. Amazon SQS sends the response right away, even if the query found no messages\. 

With *long polling*, the [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html) request queries all of the servers for messages\. Amazon SQS sends a response after it collects at least one available message, up to the maximum number of messages specified in the request\. Amazon SQS sends an empty response only if the polling wait time expires\. 

The following sections explain the details of short polling and long polling\.

**Topics**
+ [Consuming messages using short polling](#sqs-short-polling)
+ [Consuming message using long polling](#sqs-long-polling)
+ [Differences between long and short polling](#sqs-short-long-polling-differences)

## Consuming messages using short polling<a name="sqs-short-polling"></a>

When you consume messages from a queue using short polling, Amazon SQS samples a subset of its servers \(based on a weighted random distribution\) and returns messages from only those servers\. Thus, a particular [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html) request might not return all of your messages\. However, if you have fewer than 1,000 messages in your queue, a subsequent request will return your messages\. If you keep consuming from your queues, Amazon SQS samples all of its servers, and you receive all of your messages\.

The following diagram shows the short\-polling behavior of messages returned from a standard queue after one of your system components makes a receive request\. Amazon SQS samples several of its servers \(in gray\) and returns messages A, C, D, and B from these servers\. Message E isn't returned for this request, but is returned for a subsequent request\.

![\[Message Sampling using Short (Standard) Polling\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/ArchOverview_Receive.png)

## Consuming message using long polling<a name="sqs-long-polling"></a>

When the wait time for the `ReceiveMessage` API action is greater than 0, *long polling* is in effect\. The maximum long polling wait time is 20 seconds\. Long polling helps reduce the cost of using Amazon SQS by eliminating the number of empty responses \(when there are no messages available for a `[ReceiveMessage](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html)` request\) and false empty responses \(when messages are available but aren't included in a response\)\. For information about enabling long polling for a new or existing queue using the AWS Management Console or the AWS SDK for Java \(and the `[CreateQueue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_CreateQueue.html)`, `[SetQueueAttributes](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SetQueueAttributes.html)`, and `[ReceiveMessage](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html)` actions\), see the [Tutorial: Configuring long polling for an Amazon SQS queue](sqs-configure-long-polling-for-queue.md) tutorial\. For best practices, see [Setting up long polling](working-with-messages.md#setting-up-long-polling)\.

Long polling offers the following benefits:
+ Eliminate empty responses by allowing Amazon SQS to wait until a message is available in a queue before sending a response\. Unless the connection times out, the response to the `ReceiveMessage` request contains at least one of the available messages, up to the maximum number of messages specified in the `ReceiveMessage` action\.
+ Eliminate false empty responses by querying all—rather than a subset of—Amazon SQS servers\.
**Note**  
You can confirm that a queue is empty when you perform a long poll and the `ApproximateNumberOfMessagesDelayed`, `ApproximateNumberOfMessagesNotVisible`, and `ApproximateNumberOfMessagesVisible` metrics are equal to 0 at least 1 minute after the producers stop sending messages \(when the queue metadata reaches eventual consistency\)\. For more information, see [Available CloudWatch metrics for Amazon SQS](sqs-available-cloudwatch-metrics.md)\.
+ Return messages as soon as they become available\.

## Differences between long and short polling<a name="sqs-short-long-polling-differences"></a>

Short polling occurs when the `WaitTimeSeconds` parameter of a `[ReceiveMessage](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html)` request is set to `0` in one of two ways:
+ The `ReceiveMessage` call sets `WaitTimeSeconds` to `0`\.
+ The `ReceiveMessage` call doesn’t set `WaitTimeSeconds`, but the queue attribute [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SetQueueAttributes.html](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SetQueueAttributes.html) is set to `0`\.