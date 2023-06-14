# Understanding AMQP with AsyncAPI

## Introduction
### AMQP – Advanced Message Queuing Protocol
<p>AMQP is a platform-neutral messaging protocol that allows communication between applications using middleware brokers. 
 Similar to other event-driven architectures, AMQP makes use of publishers and consumers and can have its entities defined within the application.</p>
 
### Core Concepts
* **Queues** - It is an entity that stores messages to be consumed by an application.
* **Exchanges** - Messages are first published to an exchange where it is routed to the required queue using a routing key. This exchange type can differ based on the routing topology used.
* **Bindings** - These are the rules used by exchanges to determine how to route a message.

## Create AsyncAPI document
## Code Generation
### Setting up RabbitMQ
### Generate Code
### Validate Output
