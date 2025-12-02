# community/kafka

Commands to interact with Apache Kafka using the Kafka CLI

This package provides thin aux4 command wrappers around the Apache Kafka command-line tools (kafka-topics, kafka-console-producer, kafka-console-consumer, kafka-consumer-groups). It makes common Kafka admin and messaging tasks available through concise aux4 command paths, with sensible defaults (bootstrap server defaults to localhost:9092). Use it to list, create, describe, and delete topics; produce and consume messages; and manage consumer groups from your terminal.

## Installation

```bash
aux4 aux4 pkger install community/kafka
```

## System Dependencies

This package requires system dependencies. You need to have one of the following system installers:

- [brew](/r/public/packages/aux4/system-installer-brew)

For more details, see [system-installer](/r/public/packages/aux4/pkger/commands/aux4/pkger/system).

## Quick Start

List all topics on a Kafka broker running on the default bootstrap server (localhost:9092):

```bash
aux4 kafka topic list
```

This runs kafka-topics against the default Bootstrap server (localhost:9092) and prints a list of topics.

## Topics — manage Kafka topics

Manage Kafka topics: list existing topics, create new topics, inspect topic details, and delete topics.

To list topics (uses bootstrapServer default of localhost:9092):

```bash
aux4 kafka topic list
```

To create a topic named my-topic with 3 partitions and replication factor 1:

```bash
aux4 kafka topic create my-topic --partitions 3 --replicationFactor 1
```

This invokes kafka-topics --create with the provided topic name and options. The topic name is a positional argument (arg: true). If you omit --partitions or --replicationFactor, the command uses the defaults defined in the package (partitions: 1, replicationFactor: 1).

To describe a topic to see partition and replica assignments:

```bash
aux4 kafka topic describe my-topic
```

To delete a topic:

```bash
aux4 kafka topic delete my-topic
```

For command reference see [aux4 kafka topic list](./commands/kafka/topic/list) and the related command pages under ./commands/kafka/topic.

## Messages — produce and consume messages

Produce messages to a topic (reads from stdin), or consume messages from a topic with optional start-from-beginning and consumer group support.

To produce one message to my-topic:

```bash
echo "hello world" | aux4 kafka message produce my-topic
```

This pipes the message into kafka-console-producer pointed at the configured bootstrap server (default: localhost:9092).

To consume messages from my-topic from the end (default):

```bash
aux4 kafka message consume my-topic
```

To consume messages from the beginning of the topic:

```bash
aux4 kafka message consume my-topic --fromBeginning
```

To consume from the beginning while specifying a consumer group ID:

```bash
aux4 kafka message consume my-topic --fromBeginning --group my-group
```

The --fromBeginning flag maps to the kafka-console-consumer --from-beginning option; the --group option maps to --group ${group}.

For command reference see [aux4 kafka message consume](./commands/kafka/message/consume).

## Consumer groups — list, describe, delete

You can list consumer groups, describe a specific group's partition assignment and offsets, and delete a group.

To list all consumer groups:

```bash
aux4 kafka group list
```

To describe a consumer group named my-group:

```bash
aux4 kafka group describe my-group
```

To delete a consumer group:

```bash
aux4 kafka group delete my-group
```

For command reference see [aux4 kafka group list](./commands/kafka/group/list).

## Examples

### Create a topic, produce a message, then consume it

Create a topic named demo-topic with one partition and replication factor 1, send a message, then consume from the beginning:

```bash
aux4 kafka topic create demo-topic --partitions 1 --replicationFactor 1
echo "demo message" | aux4 kafka message produce demo-topic
aux4 kafka message consume demo-topic --fromBeginning
```

This sequence creates the topic (if not present), writes "demo message" to the topic, and then starts a consumer that reads from the beginning so you can see the message you just produced.

### Describe topic partitions and replicas

After creating a topic, inspect its partition and replica assignments:

```bash
aux4 kafka topic describe demo-topic
```

This runs kafka-topics --describe for the topic and shows partition leadership and replica sets.

### Consume using a consumer group

Start consuming messages with a named consumer group (useful for offset tracking across restarts):

```bash
aux4 kafka message consume demo-topic --group demo-consumer-group
```

If you want to reprocess from the beginning with the same group, include --fromBeginning.

### List and inspect consumer groups

List all consumer groups and describe a specific group:

```bash
aux4 kafka group list
aux4 kafka group describe demo-consumer-group
```

These commands use kafka-consumer-groups against the configured bootstrap server.

## Configuration

This package relies on Kafka CLI tools installed on your system and connects to a Kafka bootstrap server. The following variable controls the target server and has a sensible default:

- bootstrapServer — Kafka bootstrap server address (default: localhost:9092)

When needed, pass a different bootstrap server via the --bootstrapServer flag, for example:

```bash
aux4 kafka topic list --bootstrapServer my.kafka.host:9092
```

Topic names and consumer group IDs are provided as positional arguments where indicated (arg: true).

## License

This package does not specify a license. See [LICENSE](./license) for details.
