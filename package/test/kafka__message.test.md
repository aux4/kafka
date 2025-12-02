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
printf "Message 1\nMessage 2\nMessage 3" | aux4 kafka message produce test-aux4-message-topic
```

```expect
```
