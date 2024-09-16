# DDS Bindings

This document defines how to describe DDS-specific information in AsyncAPI.

It applies to DDS (Data Distribution Service) version 1.4, covering core concepts such as DomainParticipant, Publisher, Subscriber, DataWriters, DataReaders, Topics, and QoS policies.

<a name="version"></a>

## Version

Current version is `0.1.0`.

<a name="server"></a>

## Server Binding Object

This object contains information about the server (DomainParticipant) representation in DDS.

##### Fixed Fields

Field Name | Type | DDS Versions | Description
---|:---:|:---:|---|
`participantId` | string | 1.4 | The identifier of the DomainParticipant, representing the local membership of the application in a domain.
`qosPolicies` | object | 1.4 | QoS policies applied to the DomainParticipant, controlling its behavior, including reliability, durability, and liveliness.
`discovery` | object | 1.4 | Discovery options for the DomainParticipant, including whether automatic discovery of other participants in the domain is enabled.
`bindingVersion` | string | | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Examples

```yaml
servers:
  ddsServer:
    bindings:
      dds:
        participantId: domain_participant_1
        qosPolicies:
          reliability: reliable
          durability: transient
        discovery:
          autoDiscovery: true
        bindingVersion: 0.1.0
```

<a name="channel"></a>


## Channel Binding Object

This object represents DDS topics, which are used for publish/subscribe communication between DataWriters and DataReaders.

##### Fixed Fields

Field Name | Type | DDS Versions | Description
---|:---:|:---:|---|
`topicName` | string | 1.4 | The name of the topic for message exchange between publishers and subscribers.
`dataType` | object | 1.4 | The data type associated with the topic, defining the structure of messages.
`qosPolicies` | object | 1.4 | QoS policies applied to the topic, such as durability, latency, deadline, and reliability.
`bindingVersion` | string | | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Examples

```yaml
channels:
  sensor/data:
    bindings:
      dds:
        topicName: SensorDataTopic
        dataType: SensorReading
        qosPolicies:
          reliability: reliable
          durability: volatile
          deadline: 200ms
        bindingVersion: 0.1.0
```


<a name="operation"></a>

## Operation Binding Object

This object describes the DDS operations for publishing and subscribing, mapping DataWriter and DataReader operations to AsyncAPI's publish/subscribe.

##### Fixed Fields

Field Name | Type | Applies To | DDS Versions | Description
---|:---:|:---:|:---:|---|
`qos` | object | Publish, Subscribe | 1.4 | Defines QoS policies for the operation, including reliability and latency.
`dataWriter` | string | Publish | 1.4 | The DataWriter associated with the topic, responsible for publishing data.
`dataReader` | string | Subscribe | 1.4 | The DataReader associated with the topic, responsible for receiving data.
`reliability` | string | Publish, Subscribe | 1.4 | Specifies the reliability of the message delivery (e.g., best-effort or reliable).
`lifespan` | string | Publish | 1.4 | Specifies how long a message should remain valid.
`bindingVersion` | string | | | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Examples

```yaml
channels:
  sensor/data:
    publish:
      bindings:
        dds:
          qos:
            reliability: reliable
            lifespan: 5s
          dataWriter: sensor_writer
          bindingVersion: 0.1.0
```
```yaml
channels:
  sensor/data:
    subscribe:
      bindings:
        dds:
          qos:
            reliability: best-effort
          dataReader: sensor_reader
          bindingVersion: 0.1.0
```


<a name="message"></a>

## Message Binding Object

This object defines how DDS messages are represented in AsyncAPI. Each message has a data type and QoS settings that control its behavior.

##### Fixed Fields

Field Name | Type | DDS Versions | Description
---|:---:|:---:|---|
`dataType` | object | 1.4 | Defines the type of the message payload, corresponding to the data type used by DataWriters and DataReaders.
`keyFields` | string | 1.4 | An array of field names used to uniquely identify instances of data.
`bindingVersion` | string | | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Examples

```yaml
channels:
  sensor/data:
    subscribe:
      message:
        bindings:
          dds:
            dataType: SensorReading
            keyFields:
              - sensorId
            bindingVersion: 0.1.0
```

