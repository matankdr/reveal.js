# Virtual Threads


## Async at Wix

| I/O Bound | Stability | The Goal |
|---|---|---|
| Wix services spend most of their time waiting on Databases, APIs, and Caches. | Non-blocking operations are crucial to keep the system responsive under load. | Handling high throughput with minimal resources (CPU/RAM). |

---

## ExecutorService & Future

### Thread Pools

We reuse resources instead of creating new OS threads for every request.

### Future

A placeholder for a result that hasn't arrived yet. Calling `.get()` **blocks the current thread** until completion.

```java
// Submit task to pool
Future<String> future = pool.submit(this::fetchData);

// Do other work...

// Blocks until result is ready
String result = future.get();
```

---

## Virtual Threads (Java 21)

- **Lightweight**: managed by the JVM, not the OS kernel
- **Mount/Unmount**: VTs "mount" on OS threads only when executing CPU tasks
- **The Yield**: when waiting for I/O (DB, HTTP), the VT "unmounts", freeing the OS thread to handle other requests


## Scaling with Virtual Threads

- **1M+** concurrent tasks on a single machine
- **KB** per thread (vs MB for OS threads)
- **Drop-in** — easy migration from platform threads

```java
// One line — same API as before
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    executor.submit(() -> fetchData());
}
```
