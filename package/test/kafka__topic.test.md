# aux4 kafka topic

```beforeAll
aux4 kafka topic delete test-aux4-topic-1 2>/dev/null || true
aux4 kafka topic delete test-aux4-topic-2 2>/dev/null || true
```

```afterAll
aux4 kafka topic delete test-aux4-topic-1 2>/dev/null || true
aux4 kafka topic delete test-aux4-topic-2 2>/dev/null || true
```

## create topic

```execute
aux4 kafka topic create test-aux4-topic-1
```

```expect
Created topic test-aux4-topic-1.
```

## create topic with partitions

```execute
aux4 kafka topic create test-aux4-topic-2 --partitions 3
```

```expect
Created topic test-aux4-topic-2.
```

## list topics

```execute
aux4 kafka topic list | grep test-aux4-topic
```

```expect
test-aux4-topic-1
test-aux4-topic-2
```

## describe topic

```execute
aux4 kafka topic describe test-aux4-topic-1
```

```expect:partial
Topic: test-aux4-topic-1
```

## describe topic with partitions

```execute
aux4 kafka topic describe test-aux4-topic-2
```

```expect:partial
PartitionCount: 3
```

## delete topic

```execute
aux4 kafka topic delete test-aux4-topic-1
```

```expect
```

## delete second topic

```execute
aux4 kafka topic delete test-aux4-topic-2
```

```expect
```

## verify topics deleted

```execute
aux4 kafka topic list | grep test-aux4-topic || echo "no topics found"
```

```expect
no topics found
```
