# aux4 kafka message

```beforeAll
aux4 kafka topic delete test-aux4-message-topic 2>/dev/null || true
aux4 kafka topic create test-aux4-message-topic
```

```afterAll
aux4 kafka topic delete test-aux4-message-topic 2>/dev/null || true
```

## produce message

```execute
echo "Hello World" | aux4 kafka message produce test-aux4-message-topic
```

```expect
```

## produce multiple messages

```execute
printf "Message 1\nMessage 2\nMessage 3" | aux4 kafka message produce test-aux4-message-topic && sleep 2
```

```expect
```

## consume messages from beginning

```timeout
10000
```

```execute
aux4 kafka message consume test-aux4-message-topic --fromBeginning --timeout 5000 2>&1 | grep -v "ERROR\|TimeoutException\|Processed"
```

```expect:partial
Hello World
```

```expect:partial
Message 1
```

## stream messages from beginning

```timeout
10000
```

```execute
aux4 kafka message stream test-aux4-message-topic --fromBeginning --timeout 5000 2>&1 | grep -v "ERROR\|TimeoutException\|Processed"
```

```expect:partial
Hello World
```

```expect:partial
Message 2
```
