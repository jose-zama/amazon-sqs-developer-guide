# Tutorial: Deleting an Amazon SQS queue<a name="sqs-delete-queue"></a>

If you don't use an Amazon SQS queue \(and don't foresee using it in the near future\), it is a best practice to delete it from Amazon SQS\. In this tutorial you'll learn how to delete a queue\.

**Note**  
You can delete a queue even when it isn't empty\. If you want to delete the messages in a queue but not the queue itself, you can [purge the queue](sqs-purge-queue.md)\.  
By default, a queue retains a message for four days after it is sent\. You can configure a queue to retain messages for up to 14 days\.

**Topics**
+ [AWS Management Console](#delete-queue-console)
+ [AWS SDK for Java](#delete-queue-java)

## AWS Management Console<a name="delete-queue-console"></a>

1. Sign in to the [Amazon SQS console](https://console.aws.amazon.com/sqs/)\.

1. From the queue list, select a queue\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-tutorials-sending-message-to-queue-select-queue.png)

1. From **Queue Actions**, select **Delete Queue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-tutorials-deleting-queue-delete-queue.png)

   The **Delete Queues** dialog box is displayed\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-tutorials-deleting-queue-delete-queue-dialog-box.png)

1. Choose **Yes, Delete Queue**\.

   The queue is deleted\.

## AWS SDK for Java<a name="delete-queue-java"></a>

 The following example uses the AWS Java SDK\. To install and set up the SDK, see [Set up the AWS SDK for Java](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-install.html) in the *AWS SDK for Java Developer Guide*\.

Before you run the example code, configure your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html) in the *AWS SDK for Java Developer Guide*\. 

**Note**  
This action is identical for standard and FIFO queues\.

**To delete a queue**

1. Copy the [standard queue example program](standard-queues-getting-started-java.md) or the [FIFO queue example program](FIFO-queues-getting-started-java.md)\.

   The following section of the code deletes the queue:

   ```
   // Delete the queue
   System.out.println("Deleting the test queue.\n");
   sqs.deleteQueue(new DeleteQueueRequest(myQueueUrl));
   ```

1. Compile and run the example\.

   The queue is deleted\.