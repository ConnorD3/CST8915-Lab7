# CST8915 Lab 7: Introduction to Kubernetes Basics

**Student Name**: Connor Dickson
**Student ID**: 041085826
**Course**: CST8915 Full-stack Cloud-native Development
**Semester**: Fall 2026

---

## Demo Video

🎥 [Watch Demo Video](https://youtu.be/IpgAxIVHT4I)

---

## RabbitMQ Configuration issues

RabbitMQ is a stateful service, this is because it state of the order queues is stored within the service rather than being removed from the services after being processed. Without the addition of persistent storage such as a database, RabbitMQ must hold on to queue data sent from order-service indefinitely. In the case of this deployment, when the RabbitMQ pod is deleted, it is immediately replaced by a new instance of the RabbitMQ service, as the yaml files specifies there needs to always be one replica of RabbitMQ running. The problem with deleting RabbitMQ in the current configuration is that, because the service is stateful, the queue data from the deleted instance is also lost during deletion, and as a result the order history of end users is also lost. One option to solve the issues presented by RabbitMQ would be the RedPanda streaming data service. While its considered stateful, by default it it updates an append-only log in the underlying storge, so that there is no data loss in the event of the cluster failing. Redpanda also replicates message data across multiple, lightweight nodes to ensure that if a single broker instance is deleted, that another instance can quickly take its job and already have a fully up to date queue. A managed solution to RabbitMQ could be Amazon SQS, which stores message data across multiple isolated servers, so that if an instance fails or is deleted, messages persist outside of the nodes themselves. The issues with RabbitMQ could also probably be solved by adding a more persistent storage method for messages such as a database. 

Azure Service Bus can solve the issues presented by RabbitMQ as message data can be made reduncent through the use of Geo-Replication, meaning that any deletion,crash, outage or disaster in one region will not cause permanent loss of message data. Azure service bus is also very scalable, maintaining performance better than RabbitMQ under a heavier load.

---

## Acknowledgments

*Amazon. (n.d.). What is amazon simple queue service? AWS. Retrieved https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html*

*Larson, C. (2026, November 12). Redpanda as a RabbitMQ alternative. Redpanda. https://www.redpanda.com/blog/rabbitmq-alternative-redpanda-vs-rabbitmq*

*Microsoft. (2026, January 27). Reliability in azure service bus. Microsoft Learn. https://learn.microsoft.com/en-us/azure/reliability/reliability-service-bus*

