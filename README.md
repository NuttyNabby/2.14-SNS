# 2.14-SNS
1. Does SNS guarantee exactly-once delivery to subscribers?
No, Amazon SNS (Simple Notification Service) does not guarantee exactly-once delivery. SNS follows an at-least-once delivery model, meaning a message may be delivered more than once to a subscriber. Duplicate messages can occur due to network retries, which is why subscribers must implement idempotency to handle duplicate messages.

However, Amazon SNS FIFO (First In, First Out) topics, which integrate with Amazon SQS FIFO queues, provide strict ordering and deduplication to ensure exactly-once processing.

2. What is the purpose of the Dead-letter Queue (DLQ)?
- A Dead-letter Queue (DLQ) is used to handle failed message deliveries in Amazon SQS, SNS, and EventBridge. The primary purposes of a DLQ are:

- Message retention: Captures messages that could not be processed or delivered successfully.

- Debugging: Helps in identifying issues with message processing by analyzing failed messages.

- Error handling: Prevents lost messages by keeping them in a queue for later analysis or reprocessing.

- System reliability: Improves fault tolerance by preventing failed messages from clogging the main queue.

In SNS, DLQs are used to store messages that could not be delivered to a subscribed endpoint (e.g., Lambda, SQS, HTTP/S).

3. How would you enable a notification to your email when messages are added to the DLQ?
To receive an email notification when messages are added to the DLQ, follow these steps:

- Create an Amazon SNS topic:

- Go to the SNS console.

- Click Create topic → Choose Standard or FIFO.

- Name the topic (e.g., DLQ-Alerts).

- Subscribe your email to the SNS topic:

- In the SNS topic, click Create subscription.

- Choose Protocol: Email and enter your email address.

- Check your inbox and confirm the subscription.

- Set up an Amazon CloudWatch Alarm for DLQ:

- Go to the CloudWatch console → Alarms → Create Alarm.

- Choose Metric → SQS → Queue Metrics → ApproximateNumberOfMessagesVisible (for DLQ).

- Set a threshold (e.g., if messages > 0).

- Choose Actions → Send notification to SNS topic (select DLQ-Alerts).

Now, whenever messages are added to the DLQ, CloudWatch triggers an alert via SNS, sending an email notification.
