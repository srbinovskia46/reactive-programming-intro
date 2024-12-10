
# Reactive Programming

## What is Reactive Programming?

Reactive programming is a declarative programming paradigm that focuses on responding to changes in data and events in real-time. At its core, it revolves around three key concepts:

- **Data Streams**: A continuous flow of data objects—ranging from zero to infinite—that at undetermined intervals travel from one point to another.
- **Functional Programming**: Declarative functions used to process, transform, and manipulate the data within these streams.
- **Asynchronous Observers**: Non-blocking mechanisms that allow code to execute efficiently without waiting for events to complete.

This paradigm is designed to handle dynamic and evolving systems by enabling asynchronous and non-blocking data processing. It excels in scenarios where the system needs to react instantly to events such as user inputs, live data feeds, or messages from other systems.

---

### Visual Representation of Data Streams

![alt text](https://miro.medium.com/v2/resize:fit:720/format:webp/0*DwzgTfD03WvqAx3Z.png)

Here is a visual representation of how data might flow. Here specifically we see a data stream with numbers, but it doesn’t need to be, it could be strings, floats, custom objects... Notice how they don’t come at the same intervals. The stream doesn’t serve data in predetermined intervals. The data can be served whenever. It may also contain 0, 1 or even an infinite amount of data. Next, we see that we can transform the data. The map function was used, multiplying every number by 2 and then they were filtered to numbers only larger than the number 5. It is best practice to use separate, simple functions chained together instead of complex functions. This is one example of how a data stream looks like and how the data can be changed.

**Example of Transformation:**
- A `map` function is applied to multiply each number by 2.
- The resulting data is filtered to include only numbers greater than 5.

**Best Practice:** Use simple, separate functions chained together rather than creating complex functions.

---

### Asynchronous vs. Synchronous Processing

![alt text](https://www.matthewyancer.com/assets/images/synchronous-vs-asynchronous.webp)

Now, we saw that the data doesn’t come right away. We might have to wait a long time for the next number to come, so there is no point blocking the other processes waiting for us to retrieve the data. Also, we do not know how many numbers we might see. What this essentially means is that the system can react to changes in data, without blocking the other processes waiting for the changes to happen. This is why reactive programming focuses on being asynchronous. The graphic above shows a visual representation of how asynchronous processing looks like compared to the synchronous processing you might be used to.

**Key takeaway:** Reactive programming focuses on asynchronous processing, where operations do not block other processes while waiting for data. This approach ensures systems can react to changes in data without halting other tasks.

---

### Back Pressure

![alt text](https://tech.io/servlet/fileservlet?id=79301389043122)

The idea of back pressure takes centre stage in circumstances where data flows through interconnected components like a river. When a fast-paced event producer engages with a slower subscriber, its function becomes even more important.

In reactive systems, back pressure serves as the conductor, maintaining the digital symphony's steady tempo. It allows subscribers to let publishers know when they should pause or slow down the pace of events. By preventing downstream components from being overloaded, this orchestration balances the system's processing speed and capacity.

**Key takeaway:** In order to create applications that are resilient, scalable, and responsive to changing data streams, reactive systems use back pressure to strike a careful balance.

---

## Publish/Subscribe Pattern

The publish/subscribe pattern is essentially the heart of reactive programming, where a publisher emits a stream of data, and a subscriber listens to and processes this stream. We may have multiple subscribers to the same publisher. This flow is "push-based," meaning the data is sent to the subscriber as soon as it becomes available. The subscriber can then transform, filter, or otherwise manipulate the data in real time. They may also opt out of listening for updates from the publisher. 

**In short:** The publish/subscribe pattern is central to reactive programming. In this model:
- A **publisher** emits a data stream.
- One or more **subscribers** listen to and process the data stream in real-time.

The flow is "push-based," meaning data is sent to subscribers as soon as it's available. Subscribers can transform, filter, or otherwise manipulate the data and can also unsubscribe from updates when needed.

### Difference from Streams API

Some of you might look at reactive code written in Java or Kotlin and it might seem the same as the Streams API. While here we are not going to go into detail in the specific implementations of the Rx libraries, we are going to mention that while Java Streams still represent a “stream” of data, usually the data is already in memory and the transformation happens synchronously, while here we do not have the data in memory, and everything happens asynchronously. Also, a big difference from the Streams API and the observer pattern is that here there are 3 channels:
- **Data channel:** This is where the data 
- **Error channel:** When there is an error it is sent through this channel. 
- **Complete channel:** This indicates when there is no more data coming through.

These channels let us very easily get the data, handle errors if there are any and unsubscribe if the stream is completed. 


**In short:** While the Streams API may seem similar, reactive programming differs in these ways:
1. Data in reactive programming is not preloaded into memory and is handled asynchronously.
2. Reactive streams utilize three distinct channels:
   - **Data Channel**: Where data arrives.
   - **Error Channel**: For handling errors.
   - **Complete Channel**: Indicates when the stream has finished.

These channels make it easier to process data, handle errors, and unsubscribe when needed.

---

## Where is it Used?

Reactive programming is widely used in scenarios requiring real-time data processing:

- **User Interactions**: Ensures responsive apps by reacting instantly to user inputs like swipes or taps.
- **Real-Time Data Feeds**: For applications like stock market updates, IoT devices, or live news feeds.
- **Messaging Systems**: Efficiently handles event-driven architectures.
- **Media Streaming**: Supports real-time chat and video streaming.

By managing asynchronous data streams, reactive programming ensures scalability, responsiveness, and resilience in modern applications.

---

## How to Use Reactive Programming

Below are examples to illustrate the use of reactive programming in practice.

### Kotlin Example

```kotlin
dataStream
    .map { it * 2 }
    .filter { it > 5 }
    .subscribe(
        onNext = { println("Data: $it") },
        onError = { println("Error: $it") },
        onComplete = { println("Stream complete") }
    )
```

Here:
- **`map`** and **`filter`** are functional programming constructs for data transformation.
- **`subscribe`** listens to three channels:
    - Data channel.
    - Error channel.
    - Complete channel.

### Imperative Code Comparison

The equivalent imperative code is verbose and harder to maintain:

```kotlin
val result = mutableListOf<Int>()
for (item in dataStream) {
    val transformed = item * 2
    if (transformed > 5) {
        result.add(transformed)
    }
}
result.forEach { println(it) }
```

Declarative programming offers a cleaner, more efficient way to handle such transformations.

---

### Combining Multiple Streams

Reactive programming allows combining multiple streams and handling errors gracefully:

```kotlin
combinedStream
    .onErrorResume { println("Error occurred, stream ended") }
    .subscribe { println(it) }
```

This ensures errors don’t crash the application and provides proper handling while completing the stream.

## Practical Use Cases and Considerations

Reactive programming shines in scenarios that require real-time data processing and responsiveness. Below are examples demonstrating where and how you might use reactive streams effectively. Each example showcases practical implementations and highlights the strengths of reactive programming.

### Real-time stock price updates

```kotlin
// Simulate a real-time stock price stream  
val stockUpdates = Observable.interval(1, TimeUnit.SECONDS)  
    .map { 
    val stockSymbol = listOf("GOOG", "AMZN", "MSFT").random() 
  val stockPrice = Random.nextInt(50, 200)  
  StockUpdate(stockSymbol, stockPrice, System.currentTimeMillis())
    }  
  // Print the original data  
  .doOnNext { println("Received: $it") }  
  // Filter for stock prices greater than 100
  .filter { stockUpdate -> stockUpdate.price > 100 }  
  // Transform data  
  .map { stockUpdate -> stockUpdate.copy(symbol = "${stockUpdate.symbol} [Processed]") }  
  
// Subscribe to consume the stock updates  
stockUpdates.subscribe(  
    { stockUpdate -> println("Processed: $stockUpdate") }, // OnNext  
  { error -> println("Stream error: ${error.message}") }, // OnError  
  { println("Stream completed") } // OnComplete  
)  
  
// Keep the main thread alive to observe the output  
Thread.sleep(20000)
```

Example of output:
```
Received: StockUpdate(symbol=GOOG, price=132, timestamp=1733742176460)
Processed: StockUpdate(symbol=GOOG [Processed], price=132, timestamp=1733742176460)
Received: StockUpdate(symbol=MSFT, price=62, timestamp=1733742177458)
Received: StockUpdate(symbol=MSFT, price=91, timestamp=1733742178456)
Received: StockUpdate(symbol=MSFT, price=165, timestamp=1733742179454)
Processed: StockUpdate(symbol=MSFT [Processed], price=165, timestamp=1733742179454)
```

This example simulates a live stream of stock price updates. First we simulate the stream of incoming data for our stocks. We process them as an object called StockUpdate consisting of a stock symbol, which is the name of the company, the price and the current time the data arrived. Once we receive an update from the stream of data we print the initial state and then manipulate it. Prices are filtered to include only those above 100 and transformed by adding metadata, such as marking stocks as "processed." Subscribers react to the updates in real-time, showcasing how reactive programming can handle high-frequency data streams efficiently. This approach is ideal for scenarios like stock market tracking or other real-time monitoring systems.

### Sensor Data Processing with Reactive Programming
```kotlin
 val startTime = System.currentTimeMillis()  
  
    // Simulate making asynchronous calls to 10 APIs (e.g., fetching sensor data from a remote service)  
  val sensorDataStream = (1..10).toObservable()  
        .flatMap { getSensorDataFromApiReactive(it) }  
  .doOnNext { println(it) }  
  
  sensorDataStream.blockingLast()  
  
    val endTime = System.currentTimeMillis()  
    println("Reactive execution time: ${endTime - startTime} ms")  
}  
  
fun getSensorDataFromApiReactive(id: Int): Observable<String> {  
    // Simulating an asynchronous call to an external API  
  return Observable.just("Sensor $id: Temperature ${Random.nextInt(20, 30)}°C")  
		 // Simulate network delay
        .delay(Random.nextLong(100, 200), TimeUnit.MILLISECONDS)  
```

Example output:
```
Sensor 3: Temperature 29°C
Sensor 8: Temperature 29°C
Sensor 9: Temperature 25°C
Sensor 10: Temperature 22°C
Sensor 5: Temperature 28°C
Sensor 1: Temperature 24°C
Sensor 6: Temperature 25°C
Sensor 2: Temperature 28°C
Sensor 4: Temperature 26°C
Sensor 7: Temperature 28°C
Reactive execution time: 306 ms
```
This code snippet demonstrates using Reactive Programming to simulate fetching sensor data asynchronously for 10 sensors. It creates an observable stream of integers (`1..10`), where each number represents a different sensor ID. The `flatMap` operator is used to simulate concurrent API calls, with each call returning a random temperature (20°C to 30°C) after a random delay, mimicking network latency. The `doOnNext` operator processes and prints each emitted value, providing real-time feedback. The `blockingLast()` operator ensures the program waits for all asynchronous operations to complete before calculating and printing the total execution time. This approach showcases how reactive programming efficiently handles concurrency and asynchronous tasks while remaining scalable and readable.

### Sensor Data Processing with Traditional Blocking

```kotlin
 val startTime = System.currentTimeMillis()  
  
    // Simulate making 10 blocking calls to APIs  
  for (i in 1..10) {  
        val data = getSensorDataFromApiBlocking(i)  
        println(data)  
    }  
  
    val endTime = System.currentTimeMillis()  
    println("Blocking execution time: ${endTime - startTime} ms")  
}  
  
fun getSensorDataFromApiBlocking(id: Int): String {  
    // Simulating a blocking call to an external API  
  Thread.sleep(Random.nextLong(100, 200)) // Simulating network delay (blocking)  
  return "Sensor $id: Temperature ${Random.nextInt(20, 30)}°C"
```

Example output:
```
Sensor 1: Temperature 20°C
Sensor 2: Temperature 23°C
Sensor 3: Temperature 27°C
Sensor 4: Temperature 25°C
Sensor 5: Temperature 28°C
Sensor 6: Temperature 23°C
Sensor 7: Temperature 29°C
Sensor 8: Temperature 23°C
Sensor 9: Temperature 28°C
Sensor 10: Temperature 23°C
Blocking execution time: 1586 ms
```
This code simulates making 10 sequential, blocking API calls using a loop. Each call to `getSensorDataFromApiBlocking()` introduces a delay with `Thread.sleep()`, mimicking a network operation that halts the current thread until the response is received. The calls are processed one at a time, ensuring that each operation is completed before the next begins. The start and end times are recorded to calculate the total execution time, highlighting the inefficiency of blocking operations compared to concurrent or asynchronous approaches. This example contrasts with reactive programming by showcasing the slower performance and lack of scalability inherent in blocking calls.

#### Performance overview

The blocking sensor operation took **1586 ms** because each API call is sequential and blocks the thread, causing delays to accumulate. In contrast, the reactive approach only took **306 ms**, as it allows concurrent execution of API calls, utilizing non-blocking operations to handle multiple requests efficiently.

### When not to use Reactive programming
```kotlin
data class User(val id: Int, val name: String, val email: String)  
  
class UserService {  
    fun getUserById(userId: Int): User {  
        // Simulate database call  
  println("Fetching user details for ID: $userId")  
        Thread.sleep(1000) // Simulating delay  
  return User(userId, "John Doe", "john.doe@example.com")  
    }  
}  
  
class ReportService {  
    fun generateUserReport(user: User): String {  
        // Simulate report generation  
  println("Generating report for user: ${user.name}")  
        Thread.sleep(1000) // Simulating delay  
  return "Report for ${user.name}"  
  }  
}  
  
fun main() {  
    val userService = UserService()  
    val reportService = ReportService()  
  
    // Fetch user details  
  val user = userService.getUserById(1)  
  
    // Generate user report  
  val report = reportService.generateUserReport(user)  
  
    println(report)  
}
```

This example demonstrates a simple use case where reactive programming is not necessary. The code simulates fetching user details from a database and generating a report, both of which involve sequential, blocking operations. Since the operations depend on each other (first fetching the user, then generating the report), using reactive programming here would introduce unnecessary complexity. In such cases, where operations are inherently synchronous and there is no need for parallelism or asynchronous execution, a straightforward, blocking approach is more efficient and easier to maintain.