# Messanging Guarnateed Order Patterns Ideas

Processing messages in sequence in a distributed system is difficult, especially when message processing times vary. Below some ideas to maintain message order.

## Sequencing Identifier with Buffer Reordering
Give each message a sequencing identity to indicate its order. When messages are processed and ready for the next queue, a re-ordering buffer can store them until all preceding messages are available. 
If message B is ready before A, it will wait in the re-ordering buffer until A is ready. However, this may increase latency.

## Single-Threaded Processing
Processing messages on a single thread may be best if order is most important. 
This ensures messages are processed in sequence, but it limits scalability and may not be suited for large throughput.

## Partitioning by Key
If a key or attribute defines order, you can divide processing by key. 
All messages with the same key are processed by the same thread or consumer, maintaining order. This permits parallel processing across keys while retaining key order.
Not all messaging systems offer ordered processing. 
For instance, Apache Kafka guarantees partition ordering. 

## Leverage Sequence Number
The message can benefit from a sequence number. Processing units would then verify this number and hold off sending the message until the gap is filled (e.g., processing 4 when they haven't processed 3).
Step Functions or Similar Workflow Tools such as AWS Step Functions, can help AWS users manage multistep workflows with ordering requirements. 
We can model processing stages as a state machine to maintain order.

## Mulesoft Specific Messanging Order Ideas

In MuleSoft with numerous employees, message ordering can be difficult, especially when processing durations vary. 

### Single Ordered Processing Worker

If rigorous sequencing is needed, route specified messages through a single worker. This ensures messages are processed in sequence, but it may slow throughput.

### Sequence Number/Correlation ID
Give our messages a sequence number or correlation ID and build logic in the downstream system that consumes them from the FIFO queue to reorder them. 
If a message arrives out of sequence, the consuming system can buffer it until the previous messages arrive.

### Partitioning by Key
If ordering is only important for specific message groups, split processing by a key that determines the ordering requirement. 
All messages with the same key would be processed by the same worker, maintaining order.

### Ordering Component Custom
Create a bespoke message-reordering component. This component could be in our Mule application(upstream) or a downstream service that consumes FIFO queue messages.

# Proposals



## Create upstream custom ordering component ( microservice )

Creating an upstream custom ordering component in our architecture gives us fine-grained message sequencing control. 
The new microservice will subscribe to a new intermediate queue.

First, assign sequence identifiers
Initial messages should be assigned a sequence identification to indicate their order. This could be an incrementing number or a group identifier and sequence number inside that group if messages need to be arranged individually.

Step 2: Publish Intermediate Queue
Send messages with sequence identifiers to an intermediate queue. A distributed messaging system like Kafka or RabbitMQ can handle the volume you expect.

Third: Implement Ordering Microservice
Subscribe to the intermediate queue and order with a microservice:

### Ordering Steps

- Buffer and Reorder Messages: The microservice buffers and reorders messages by sequence number.
- Put Sequence Numbers: Assign sequence numbers to system-entry messages. This number determines message processing order.
- Buffer or Reorder Queue: Create a buffer or temporary queue to hold messages until they're ordered.
- This buffer stores processed messages, and when each expected message arrives, you can send the ordered sequence to the next step.
- Handling Timeouts: To handle delayed or missing messages, use timeouts. You may choose out-of-order processing after a timeout or use error handling.
- Keeping the Buffer: We may need to persist the buffer to a database or other durable storage to survive system restarts or failures without losing ordering, depending on dependability needs.
- Send to FIFO Queue: The microservice sends messages to the FIFO queue for processing once they are in order.
- Implement Monitoring: Track microservice performance and behaviour to identify issues and bottlenecks.
- Scale and Monitor Microservice: make the microservice stateless for simple scaling. This may include partitioning messages for parallel processing, depending on the message broker.
- Test Under Different Conditions: Make that the system works under out-of-order messages, system failures, and excessive load.
- Performance Impact: A new microservice and queues add complexity and latency, so you'll need to test to meet performance needs.



### The ordering mechanism 


- Buffering: Keep a buffer for each message group to order. If all messages are sequential, you'll have one buffer. Instead, you may have buffers for each group.
- Retrieve Messages: Get buffer messages as they arrive.
- Sort by sequence: Sort messages by sequence identifiers.
If a message is missing from the sequence (e.g., 1, 2, 4, 5, but 3 is absent), store the subsequent messages in the buffer until the missing message comes.
- Multithreading: If possible, parallelize buffers. This lets the system process numerous sequences simultaneously.
- Consider persisting buffer state to a fast storage system like Redis to handle restarts and failures without losing order.
- Send Ordered Messages to FIFO Queue

Ordering logic becomes more complicated when you don't know how many messages you'll get or if some are missing. There are numerous ways to solve this problem:

- Time-Based Windowing:
Time-based windowing determines FIFO message timing. Say, you could:
Set a Time Window: Give each message sequence 10 seconds.
Buffer Messages: Sort messages by sequence number as they arrive inside the time window.
At the conclusion of the time window, publish all buffered messages in order to the FIFO, even if any are missing.

- Pattern-Based Dynamic Waiting
Based on patterns, you can dynamically select how long to wait if messages generally arrive within a given timeframe:

Analyse Arrival timings: Use the sequence's prior message arrival timings to estimate how long you'll wait for missing messages.
This analysis can be used to define a dynamic timeout for missing communications.
If the missing message doesn't arrive within the timeout, publish the available messages to the FIFO.

3. Handling Progressively:
Buffering and progressive publication are possible:

As messages come, publish them to the FIFO until one is missing.
Hold following messages in a buffer until the missing message arrives or a timeout occurs.
If the missing message arrives, publish it in order with the buffered messages. If timeouts occur, publish buffered messages without the missing one.
4. Acknowledge or Feedback:
Implement a feedback mechanism from the message sender or another portion of the system to deliver expected message details or acknowledge receipt and processing:

Request Information: Ask the sender about message sequence if possible.
Use Acknowledgements: If the sender acknowledges receipt or processing, use this to identify when all expected messages have been received.
5. Monitoring and Warning:
These tactics should be combined with monitoring and alerting to tell you of persistent message missing or other irregularities.

Considerations:
Balancing Correctness and Latency: The technique must balance ordering and latency. Time-based windowing and dynamic waiting add delay, therefore adapt them to the system's behaviour and needs.
Handling Missing Messages: If missing messages are widespread or have major system ramifications, you may need additional logic or manual intervention.
Testing: Run the system through several scenarios to make sure it fulfils your performance and correctness criteria.



