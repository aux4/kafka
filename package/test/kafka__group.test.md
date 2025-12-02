# aux4 kafka group

```beforeAll
aux4 kafka topic delete test-aux4-group-topic 2>/dev/null || true
aux4 kafka topic create test-aux4-group-topic
echo "test message" | aux4 kafka message produce test-aux4-group-topic
sleep 1
```

```afterAll
aux4 kafka group delete test-aux4-group 2>/dev/null || true
aux4 kafka topic delete test-aux4-group-topic 2>/dev/null || true
```

## create consumer group

```timeout
15000
```

```execute
kafka-console-consumer --bootstrap-server localhost:9092 --topic test-aux4-group-topic --from-beginning --group test-aux4-group --timeout-ms 5000 2>&1 | grep -v "ERROR\|TimeoutException\|Processed"
```

```expect
test message
```

## list consumer groups

```execute
aux4 kafka group list | grep test-aux4-group
```

```expect
test-aux4-group
```

## describe consumer group

```execute
aux4 kafka group describe test-aux4-group 2>&1
```

```expect:partial
test-aux4-group
```

## delete consumer group

```execute
aux4 kafka group delete test-aux4-group 2>&1
```

```expect:partial
test-aux4-group
```
