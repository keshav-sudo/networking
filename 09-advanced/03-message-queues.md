# 📮 Message Queues

---

## 1. What is a Message Queue?

A system for async communication between services. Producer sends message, consumer processes later.

```
Producer → [Message Queue] → Consumer

Producer doesn't wait for consumer!
```

---

## 2. Popular Options

| Queue | Best For |
|-------|----------|
| **RabbitMQ** | Traditional messaging |
| **Apache Kafka** | Event streaming, high throughput |
| **Redis Pub/Sub** | Simple, low latency |
| **AWS SQS** | Managed, serverless |

---

## 3. Use Cases

```
├── Async processing (email sending)
├── Decoupling services
├── Load leveling (handle spikes)
├── Event sourcing
└── Background jobs
```

---

## 4. Code Example

```javascript
// RabbitMQ Producer
const amqp = require('amqplib');

async function send() {
  const conn = await amqp.connect('amqp://localhost');
  const channel = await conn.createChannel();
  await channel.assertQueue('tasks');
  channel.sendToQueue('tasks', Buffer.from('Hello!'));
}

// Consumer
async function receive() {
  const conn = await amqp.connect('amqp://localhost');
  const channel = await conn.createChannel();
  await channel.assertQueue('tasks');
  channel.consume('tasks', (msg) => {
    console.log(msg.content.toString());
    channel.ack(msg);
  });
}
```

---

## 5. Quick Summary

```
Message Queue: Async service communication

Benefits:
├── Decoupled services
├── Handle traffic spikes  
├── Retry failed operations
└── Scale independently

Popular: RabbitMQ, Kafka, SQS

Use for background jobs, events, notifications
```

---

*Next: [Service Discovery](./04-service-discovery.md)*
