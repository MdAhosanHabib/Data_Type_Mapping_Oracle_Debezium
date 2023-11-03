# Numeric/Number Data Type Mapping for Oracle to Kafka with Debezium 2.4 Connector
![Kafka_Oracle_Producer_DataType_Mapping](https://github.com/MdAhosanHabib/Data_Type_Mapping_Oracle_Debezium/assets/43145662/ad136d87-05d5-4499-b8ac-8cd800d37c45)

This document outlines the steps to configure the Debezium 2.4 Connector to stream data from an Oracle database into Apache Kafka. In this guide, we focus on mapping Oracle's NUMBER data type to appropriate data types in Kafka and handling numeric values.

## Prerequisites

Before proceeding, ensure that you have the following prerequisites in place:

- A running Oracle database.
- An Apache Kafka cluster.
- Debezium 2.4 Connector set up and running.

## Numeric/Number Data Type Mapping

Oracle's NUMBER data type may not have a default mapping in Debezium, which can lead to it being detected as a byte data type. This default mapping may not be ideal for decimal numbers. To handle this, you can specify the `decimal.handling.mode` parameter in your Debezium configuration.

```json
{
    "decimal.handling.mode": "string"
}
```

You can also set it to "double" based on your requirements.

## Configuration Steps
Follow these steps to configure the Debezium connector:

Check the status of the connector:
```bash
curl -X GET http://10.0.10.10:8083/connectors/inventory-connector
```

If the connector is already running, delete it:
```bash
curl -X DELETE http://10.0.10.10:8083/connectors/inventory-connector
```

Create a configuration file (e.g., connector-config.json) for your Oracle to Kafka integration:
```bash
{
    "name": "inventory-connector",
    "config": {
        "connector.class" : "io.debezium.connector.oracle.OracleConnector",
        "tasks.max" : "1",
        "topic.prefix" : "test1",
        "database.hostname" : "10.0.10.10",
        "database.port" : "1521",
        "database.user" : "debezium",
        "database.password" : "debezium56",
        "database.dbname" : "mwdb",
        "bootstrap.servers" : "10.0.10.10:9092",
        "schema.history.internal.kafka.bootstrap.servers" : "10.0.10.10:9092",
        "schema.history.internal.kafka.topic": "test",
        "table.include.list": "ahosan.TEST_DATA",
        "decimal.handling.mode": "string"
    }
}
```
Adjust the configuration as needed.

Register the connector using the configuration file:
```bash
cd /path/to/configuration/file
curl -X POST -H "Content-Type: application/json" --data @connector-config.json http://10.0.10.10:8083/connectors
```

Verify the connector's status:
```bash
curl -X GET http://10.0.10.10:8083/connectors/inventory-connector
```

## Conclusion
With the Debezium 2.4 Connector, you can successfully stream data from your Oracle database to Kafka while ensuring proper data type mapping for numeric values. Customize the configuration according to your specific requirements.

For more information on Debezium, consult the official documentation: [Debezium Documentation](https://debezium.io/documentation/reference/2.4/connectors/oracle.html)https://debezium.io/documentation/reference/2.4/connectors/oracle.html.


Regards,
Ahosan.
