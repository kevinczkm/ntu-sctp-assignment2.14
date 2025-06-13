# ntu-sctp-assignment2.14


### 1. Does SNS guarantee exactly-once delivery to subscribers?

No, **Amazon SNS does not guarantee exactly-once delivery**. It provides **at-least-once delivery** to its subscribers. This means that a message might be delivered more than once in certain conditions (e.g., retries after timeouts or errors). It's the subscriber's responsibility to handle idempotency if required.

---

### 2. What is the purpose of the Dead-letter Queue (DLQ)?

A **Dead-letter Queue (DLQ)** is used to capture messages that **cannot be successfully processed** by a target service after a defined number of retries. In the context of SNS, SQS, and EventBridge:

* DLQs help in **troubleshooting and debugging failed deliveries**.
* They prevent data loss by storing failed messages for later analysis.
* Common use cases include catching malformed messages, misconfigured endpoints, or downstream service outages.

---

### 3. How would you enable a notification to your email when messages are added to the DLQ?

To enable email notifications for DLQ messages:

1. **Create an SNS topic** for DLQ alerts.
2. **Subscribe your email address** to the SNS topic and confirm the subscription.
3. **Create a CloudWatch alarm** that triggers based on `ApproximateNumberOfMessagesVisible` metric of the DLQ.
4. **Set the CloudWatch alarm action** to publish a message to the SNS topic.
5. Result: When messages accumulate in the DLQ, CloudWatch triggers an alert → SNS topic → email notification.

