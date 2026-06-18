# Virtual Threads


## Async at Wix

| I/O Bound | Stability | The Goal |
|---|---|---|
| Wix services spend most of their time waiting on Databases, APIs, and Caches. | Non-blocking operations are crucial to keep the system responsive under load. | Handling high throughput with minimal resources (CPU/RAM). |

---

## Virtual Threads (Java 21)

- **What**: a thread *not* tied to an OS thread — scheduled by the JVM.
- **Lightweight**: managed by the JVM, not the OS kernel.
- **Mount/Unmount**: a VT "mounts" on a *carrier* OS thread only while running code.
- **The Yield**: when it waits for I/O (DB, HTTP), the VT "unmounts", freeing the carrier to run other VTs.

<div align="left" class="fragment">
<ul><li>Write simple <em>blocking</em> code — get <em>non-blocking</em> scalability.</li></ul>
</div>


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

---

## Platform vs Virtual Threads

| | Platform Thread | Virtual Thread |
|---|---|---|
| Backed by | OS kernel thread (1:1) | JVM, mounts on a carrier |
| Memory | ~1 MB stack | ~few KB, grows on demand |
| How many | Thousands | Millions |
| Creation | Expensive → **pool them** | Cheap → **one per task** |
| Blocking I/O | Blocks the OS thread | Unmounts, frees the carrier |
| Best for | CPU-bound work | I/O-bound work |

<div align="left" class="fragment">
<ul><li>Same <code>Thread</code> API, same debugger, same stack traces.</li></ul>
</div>

---

## Working With Them

```java
// Fire one off directly
Thread.startVirtualThread(() -> fetchData());

// Or build one explicitly
Thread.ofVirtual().name("worker-1").start(task);

// Preferred: one virtual thread per task
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (var id : userIds) {
        executor.submit(() -> handle(id)); // blocks freely
    }
} // closing the executor waits for all tasks
```

<div align="left" class="fragment">
<ul><li>Don't <strong>pool</strong> virtual threads — they're the cheap thing you pool <em>around</em>.</li></ul>
</div>

---

## Structured Concurrency

- Treat related tasks as **one unit of work** — fan out, then join.
- If one subtask fails, the siblings are cancelled automatically *(Preview in 21)*.

```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    var user  = scope.fork(() -> findUser(id));   // each runs
    var order = scope.fork(() -> fetchOrder(id)); // on its own VT

    scope.join().throwIfFailed();                 // wait for both
    return new Response(user.get(), order.get());
}
```

---

## Pitfalls & Best Practices

- **Pinning**: a VT inside `synchronized` can't unmount during I/O — prefer `ReentrantLock`.
- **No CPU boost**: VTs help *I/O-bound* work, not number-crunching.
- **Don't pool them**: create one per task; pooling defeats the purpose.
- **Thread-locals**: heavy `ThreadLocal` state scales poorly with millions of threads.

<div align="left" class="fragment">
<ul><li>At Wix Ninja, use the <strong>managed executor</strong> — pinning &amp; context are handled for you.</li></ul>
</div>
