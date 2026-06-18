# Concurrency Basics


## What is a Thread?

- A **thread** is an independent path of execution.
- Your program already has one: the `main` thread.
- A platform thread maps 1:1 to an **OS thread** — and costs ~**MBs** of stack.


## What is a Thread?
- `Runnable`: contains *SAM* `run` method.
- The simplest way to execute concurrent code.
```java
// A task is just a lambda (Runnable)
Runnable task = 
    () -> System.out.println("Hello from runnable");

Thread t = new Thread(task);
t.start(); // runs concurrently with main
```

<div align="left" class="fragment">
<ul><li>OS threads are expensive — remember this when we reach <strong>Virtual Threads</strong>.</li></ul>
</div>

---

## Thread Pools

- Creating an OS thread is an expensive operation.
- Prefer reusing resources 
  - Instead of creating new OS threads for every request.
  


## ExecutorService & Future

- A high-level API to run tasks **without managing threads yourself**.
- You hand it tasks; it owns the threads, the queue, and scheduling.
- Think of it as a **managed worker pool**.

```java
// A pool of 4 reusable threads
ExecutorService pool = Executors.newFixedThreadPool(4);
```


## Submitting Work

- `Runnable`: returns nothing (fire-and-forget).
- `Callable<T>`: returns a value and may throw checked exceptions.

```java
// Runnable: no result
pool.submit(() -> log.info("done"));

// Callable: produces a result
Future<String> future = pool.submit(() -> fetchData());
```


## Future

- A placeholder for a result that hasn't arrived yet. 
- Like a JS `Promise`
  - But `.get()` **blocks** instead of `.then()`.

```java
// Submit task to pool
Future<String> future = pool.submit(this::fetchData);

// Do other work...

String result = future.get();                  // blocks until ready
String quick  = future.get(2, TimeUnit.SECONDS); // or time out
boolean done  = future.isDone();               // non-blocking check
future.cancel(true);                           // attempt to cancel
```


## Don't Forget to Shutdown

- Pool threads are **non-daemon** 
    - The JVM won't exit while they're alive.
- Always shut the pool down when you're done with it.

```java
pool.shutdown();                          // stop accepting new tasks
pool.awaitTermination(30, TimeUnit.SECONDS); // wait for in-flight work
```

<div align="left" class="fragment">
<ul><li>Java 19+: <code>ExecutorService</code> is <code>AutoCloseable</code> — use try-with-resources to shut down automatically.</li></ul>
</div>