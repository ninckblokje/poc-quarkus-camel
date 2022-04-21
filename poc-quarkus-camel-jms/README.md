# poc-quarkus-camel-amqp

This PoC is for testing AMQP brokers with Quarkus and Camel using the JMS specification. It has been tested with:

- Apache Artemis
- Azure Service Bus

Using Quarkus profiles it is possible to switch between the configurations (JVM argument `-Dquarkus.profile`).

This project is almost identical to poc-quarkus-camel-amqp, however for this project the extension `quarkus-qpid-jms` is required to automatically create the connection factory.

## Apache Artemis

Active the profile `artemis` and set the following properties in a local `.env` file:

````properties
%artemis.quarkus.qpid-jms.url=amqp://[HOSTNAME]:5672
%artemis.quarkus.qpid-jms.username=[USERNAME]
%artemis.quarkus.qpid-jms.password=PASSWORD
````

A queue named `TEST_ADDRESS` is automatically created.

## Azure Service Bus

Active the profile `artemis` and set the following properties in a local `.env` file:

````properties
#%asb.quarkus.qpid-jms.url=amqps://[SB_NAMESPACE].servicebus.windows.net:5671
#%asb.quarkus.qpid-jms.username=[KEY_NAME]
#%asb.quarkus.qpid-jms.password=[KEY]
````

For `quarkus.qpid-jms.username` the name of the shared access policy key must be used. `amqps` is used as protocol to force a secure connection.

A queue named `TEST_ADDRESS` must first be manually created.
