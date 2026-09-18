# AWS SQS (Simple Queue Service)

## 1. What is SQS?

Amazon Simple Queue Service (SQS) is a fully managed AWS messaging service that allows applications to communicate asynchronously.

It stores messages in a queue until a consumer retrieves and processes them.

### Basic flow

**Producer → SQS Queue → Consumer**

- **Producer:** Application or service that sends a message.
- **Queue:** Stores messages until they are processed.
- **Consumer:** Application or service that receives and processes messages.

### Interview definition

> SQS is an AWS messaging service that stores messages in a queue until another application processes them. It helps applications communicate asynchronously.

---

## 2. Why is SQS Used?

SQS is mainly used for:

- Decoupling applications
- Asynchronous processing
- Handling traffic spikes
- Buffering tasks between applications
- Improving application reliability

### Example

An e-commerce application receives an order.

Instead of making the order application directly wait for the payment service:

**Order Application → SQS → Payment Service**

The order application places a message in SQS and continues its work. The payment service processes the message later.

---

# 3. Producer, Queue and Consumer

### Producer

The producer sends messages to the SQS queue.

Example:

```text
Order Application
       ↓
   Send Message
```

### Queue

SQS stores the message until a consumer retrieves and processes it.

### Consumer

The consumer retrieves messages from the queue and processes them.

```text
Producer → Queue → Consumer
```

Producer and consumer are roles. They do not have to be specific AWS services.

---

# 4. Standard Queue vs FIFO Queue

SQS provides two main queue types.

| Feature | Standard | FIFO |
|---|---|---|
| Throughput | Very high | Lower than Standard |
| Ordering | Best-effort ordering | Strict ordering |
| Duplicate messages | Possible | Deduplication supported |
| Main use | High-scale processing | Order-sensitive processing |

### Standard Queue

Standard queues are designed for high throughput and scalability.

They provide **at-least-once delivery**, so duplicate messages can occur.

Ordering is **best effort**.

### FIFO Queue

FIFO means **First-In, First-Out**.

FIFO queues maintain message ordering and support deduplication.

They are useful when the order of processing is important.

### Interview shortcut

**Standard → High throughput**

**FIFO → Ordering + deduplication**

---

# 5. SQS Message Lifecycle

The basic message lifecycle is:

```text
1. Producer sends message
        ↓
2. SQS stores message
        ↓
3. Consumer receives message
        ↓
4. Consumer processes message
        ↓
5. Processing successful
        ↓
6. Consumer deletes message
```

If processing fails:

```text
Consumer receives message
        ↓
Processing fails
        ↓
Message is not deleted
        ↓
Visibility Timeout expires
        ↓
Message becomes visible again
        ↓
Consumer can retry processing
```

### Important

**Receive ≠ Delete**

Receiving a message does not automatically remove it from the queue.

The consumer must explicitly delete the message after successful processing.

---

# 6. Visibility Timeout

Visibility Timeout is the amount of time for which a received SQS message remains invisible to other consumers while it is being processed.

If the consumer does not delete the message before the visibility timeout expires, the message becomes visible again.

### Why is it needed?

It helps prevent multiple consumers from processing the same message simultaneously while the first consumer is working on it.

### Interview answer

> Visibility Timeout is the time for which a received SQS message remains invisible to other consumers while it is being processed. If the message isn't deleted within that period, it becomes visible again.

### Mental shortcut

**Visibility Timeout = processing time window**

---

# 7. Message Retention Period

Message Retention Period is the amount of time SQS keeps a message in the queue.

- Default: **4 days**
- Maximum: **14 days**

After the retention period expires, SQS automatically deletes the message if it has not already been deleted.

### Mental shortcut

**Retention = how long SQS keeps the message**

Do not confuse it with Visibility Timeout.

---

# 8. Dead-Letter Queue (DLQ)

A Dead-Letter Queue is a separate SQS queue where messages that repeatedly fail processing can be moved for further investigation.

### Why use a DLQ?

- Isolate failed messages
- Investigate processing failures
- Prevent problematic messages from repeatedly cycling
- Improve troubleshooting

A DLQ is configured separately and uses a **maximum receive count** to determine when a message should be moved.

### Interview answer

> A Dead-Letter Queue is a separate queue where messages that fail to be processed repeatedly are moved for further investigation or troubleshooting.

---

# 9. Short Polling vs Long Polling

### Short Polling

The consumer requests messages and receives an immediate response.

If no messages are available, the response can be empty.

### Long Polling

SQS waits for a message for a configured period before returning a response.

This reduces empty responses and unnecessary requests.

### Interview answer

> Short polling returns a response immediately, while long polling waits for a message for a configured period, reducing empty responses and unnecessary requests.

---

# 10. SQS vs SNS

| Feature | SQS | SNS |
|---|---|---|
| Model | Queue | Pub/Sub topic |
| Message handling | Consumers retrieve messages | Messages are delivered to subscribers |
| Common use | Task processing | Notifications and fan-out |
| Consumer style | Pull | Push |

### SQS

```text
Producer → SQS Queue → Consumer
```

### SNS

```text
Publisher
    ↓
 SNS Topic
 ↙   ↓   ↘
SQS  Email  Lambda
```

SQS and SNS can also be used together.

For example:

```text
Application
     ↓
 SNS Topic
   ↙   ↘
SQS     SQS
 ↓       ↓
App A   App B
```

This allows message fan-out to multiple consumers.

---

# 11. DevOps Use Cases

SQS is commonly used for:

### 1. Asynchronous task processing

Applications can place tasks in a queue and process them asynchronously.

### 2. Application decoupling

Applications don't need to communicate directly with each other.

### 3. Handling traffic spikes

SQS can buffer messages when the consumer cannot process all requests immediately.

### 4. Reliable task processing

Messages can become visible again when processing fails, allowing retry.

---

# 12. SQS Pricing

SQS follows a usage-based, pay-as-you-go pricing model.

Pricing primarily depends on requests and can differ between Standard and FIFO queues.

AWS also provides a free usage allowance, subject to the current AWS pricing terms.

### Important

Do not memorize exact pricing numbers because AWS pricing can change.

For hands-on labs, avoid high-volume message testing.

---

# 13. SQS Practical

A Standard queue was created:

```text
devops-sqs-learning
```

The following lifecycle was demonstrated using the AWS Console:

```text
Send message
     ↓
Receive message
     ↓
Message becomes temporarily invisible
     ↓
Visibility Timeout expires
     ↓
Message becomes visible again
     ↓
Receive/process message
     ↓
Delete message
     ↓
Queue becomes empty
```

This demonstrated both the normal processing flow and the retry behavior.

---

# 14. AWS CLI Practical

AWS CLI authentication was verified using:

```bash
aws sts get-caller-identity
```

SQS queues can be listed using:

```bash
aws sqs list-queues --region ap-south-1
```

A queue was created using:

```bash
aws sqs create-queue --queue-name devops-sqs-cli-learning --region ap-south-1
```

The queue URL was retrieved using:

```bash
aws sqs get-queue-url --queue-name devops-sqs-cli-learning --region ap-south-1
```

A shell variable can be used to store the Queue URL:

```bash
QUEUE_URL=$(aws sqs get-queue-url \
  --queue-name devops-sqs-cli-learning \
  --region ap-south-1 \
  --query 'QueueUrl' \
  --output text)
```

Then verify the variable:

```bash
echo "$QUEUE_URL"
```

The queue URL can then be used by other SQS CLI operations.

---

# 15. Important SQS CLI Commands

### List queues

```bash
aws sqs list-queues --region ap-south-1
```

### Create queue

```bash
aws sqs create-queue \
  --queue-name devops-sqs-cli-learning \
  --region ap-south-1
```

### Get queue URL

```bash
aws sqs get-queue-url \
  --queue-name devops-sqs-cli-learning \
  --region ap-south-1
```

### Send a message

```bash
aws sqs send-message \
  --queue-url "$QUEUE_URL" \
  --message-body "Deploy application using CI/CD"
```

### Receive a message

```bash
aws sqs receive-message \
  --queue-url "$QUEUE_URL"
```

### Delete a message

```bash
aws sqs delete-message \
  --queue-url "$QUEUE_URL" \
  --receipt-handle "<RECEIPT_HANDLE>"
```

### Delete a queue

```bash
aws sqs delete-queue \
  --queue-url "$QUEUE_URL"
```

---

# 16. MessageId vs ReceiptHandle

These are different.

### MessageId

Identifies the message that was sent.

### ReceiptHandle

Returned when a consumer receives a message.

The **ReceiptHandle** is used when deleting the received message.

```text
Send Message
     ↓
MessageId
     
Receive Message
     ↓
ReceiptHandle
     ↓
Delete Message
```

---

# 17. Key Interview Questions

### Q1. What is SQS?

SQS is an AWS messaging service that stores messages in a queue until another application processes them. It enables asynchronous communication between applications.

### Q2. Why is SQS used?

SQS is used for application decoupling, asynchronous processing, and handling traffic spikes by buffering messages between producers and consumers.

### Q3. What is Visibility Timeout?

Visibility Timeout is the time for which a received message remains invisible to other consumers while it is being processed.

### Q4. What happens if a consumer fails to process a message?

If the message is not deleted and the visibility timeout expires, the message becomes visible again and can be processed again.

### Q5. What is a DLQ?

A Dead-Letter Queue is a separate queue used to store messages that repeatedly fail processing so they can be investigated.

### Q6. What is the difference between Standard and FIFO queues?

Standard queues provide very high throughput with best-effort ordering, while FIFO queues provide strict ordering and deduplication capabilities.

### Q7. What is the difference between SQS and SNS?

SQS is queue-based and consumers retrieve messages, while SNS is a pub/sub service that delivers messages to subscribers.

### Q8. What is Message Retention Period?

It is the amount of time SQS keeps a message in the queue. The default is 4 days and it can be configured up to 14 days.

### Q9. What is long polling?

Long polling allows SQS to wait for messages for a configured period, reducing empty responses and unnecessary requests.

### Q10. What is the difference between Receive and Delete?

Receiving a message retrieves it for processing but does not remove it permanently. The consumer must delete the message after successful processing.

---

# 18. Quick Revision

```text
SQS
│
├── Producer
│     └── Sends message
│
├── Queue
│     └── Stores message
│
├── Consumer
│     └── Receives and processes message
│
├── Visibility Timeout
│     └── Temporary invisibility during processing
│
├── Retention Period
│     └── How long SQS keeps message
│
├── DLQ
│     └── Handles repeatedly failed messages
│
├── Standard
│     └── High throughput
│
└── FIFO
      └── Ordering + deduplication
```

## Core Mental Model

**Producer → Send → SQS stores → Consumer receives → Process → Delete**

If processing fails:

**Receive → No Delete → Visibility Timeout → Message visible again → Retry**
