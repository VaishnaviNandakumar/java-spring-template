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
In this tutorial, you will learn how to implement the Smarty Lighting project using AMQP configuration. The original tutorial for this project that explains how AsyncAPI can be used in this use case can be found [here](https://www.asyncapi.com/docs/tutorials/create-asyncapi-document). The outcome of this project is a SpringBoot application that can listen and store all the messages being passed into the required queues specified by the user. <br>
Let us first start with creating an application.yml configuration file. 
<br>
<br>
![3](https://github.com/VaishnaviNandakumar/java-spring-template/assets/41518119/961e6662-864f-4e2e-9160-151121d2944f)
<br>
<br>
The attributes required for the AMQP configuration come out under the [servers](https://www.asyncapi.com/docs/concepts/server) section. In this example, [RabbitMQ](https://www.rabbitmq.com/documentation.html) is the message broker used for the implementation. Once RabbitMQ is locally configured, it will be accessible at _localhost:5672_. You can read more about how to install it for your OS [here](https://www.rabbitmq.com/download.html). 
<br>
<br>
![4](https://github.com/VaishnaviNandakumar/java-spring-template/assets/41518119/8624df73-64ba-4e95-b778-217486f41795)
<br>
<br>
The channels section of the specification is where we clearly define all the AMQP entities and their required properties. 
* Publish -
  * The operationId is used to define the name of the method to handle message publishing in the generated code. 
  * The bindings here only include an exchange as it’s the only property needed to send a message.
* Subscribe
  * The operationId in the subscriber module however is used to define the method that captures the message from the consumer side. 
  * The binding rules in this section require the queue and routingKey attribute so that the subscriber module know what to exactly look for from the exchange. <br> 
This example makes use of a single channel with one AMQP entity configuration. But what if you want to implement multiple channels? In this use case let’s say you want the information of two different streetlights both of which publish and consume messages from different queues.  In the case of multiple channels, the only difference is to add another section with the same configuration.
<br>
<br>
![5](https://github.com/VaishnaviNandakumar/java-spring-template/assets/41518119/c3f8b600-de1c-4de1-86da-a3c07b547265)
<br>
<br>
Both the single and multiple channel config files follow the same model schema for the message content. 
<br>
And with that, we have our application.yml file ready! Let’s see how we can generate a SpringBoot app out of it next.

## Code Generation

To trigger the installation via CLI follow the command given below 
```
asyncapi generate fromTemplate .\config\application.yaml @asyncapi/java-spring-template -o .\output\amqp-output
```
Let's understand the signifiance of each term here. <br>
* `asyncapi generate fromTemplate` is how you use AsyncAPI Generator via the AsyncAPI CLI.
* `asyncapi.yaml` is how you point to your AsyncAPI document and can be a URL.
* `@asyncapi/java-spring-template` is how you specify the Spring template.
* `-o` determines where to output the result.

**Start the Application** <br>
The resulting SpringBoot app would have a build.gradle file that automatically downloads and sets up all the required dependencies. Once the application successfully starts, open up a terminal to send a message. In this example, I am using `rabbitmqadmin` to publish messages to the broker through the command line. It is a CLI tool available for Windows machines. You can read more about it [here](https://www.rabbitmq.com/cli.html). <br> <br>
```
python.exe rabbitmqadmin.exe publish exchange=lightMeasuredExchange routing_key=lightMeasuredRoutingKey payload="{ \"id\":1, \"lumens\": 20}
```
<br><br>
As soon as the message is published via CLI, you can open your app to see that the springboot listener has also intercepted this message from the queue. <br><br>
![1](https://github.com/VaishnaviNandakumar/java-spring-template/assets/41518119/f3d0b1d4-ecea-433f-b01b-1a56641909a0)
<br><br>
The generated code provides a lot of flexibility for the user to decide what to do with the messages. It provides the basic template to send and receive information for an AMQP configuration but the next steps on how to utilize this data are left upto the user to build on. 

