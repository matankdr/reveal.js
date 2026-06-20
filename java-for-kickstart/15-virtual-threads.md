# Virtual Threads


## Virtual Threads (Java 21)

- **Virtual Thread**: a thread *not* tied to an OS thread 
    - scheduled by the JVM.
- **Lightweight**: managed by the JVM
  - Not by the OS kernel.
- **Mount/Unmount**: a VT "mounts" on a *carrier* OS thread only while running code.
- When VT waits for I/O (DB, HTTP), it "unmounts" 
  - Freeing the carrier to run other VTs.

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
- A subtask's lifetime is **bound to the code block** that started it.
- *(Preview in Java 21 — run with `--enable-preview`)*

<div align="left" class="fragment">
<ul><li>Like structured programming: tasks nest like <code>{ }</code> blocks — no orphans.</li></ul>
</div>


## The Problem It Solves

- With a raw executor, forked tasks become **orphans**:
  - one fails → the others keep running (wasted work, leaks)
  - cancellation and error handling are manual and easy to get wrong
- No guarantee everything finished when the method returns.

<div align="left" class="fragment">
<ul><li>Structured concurrency makes the JVM enforce the parent/child relationship.</li></ul>
</div>


## Example

```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Subtask<User>  user  = scope.fork(() -> findUser(id));   // child VT
    Subtask<Order> order = scope.fork(() -> fetchOrder(id)); // child VT

    scope.join();           // wait for ALL children
    scope.throwIfFailed();  // propagate the first error, if any

    return new Response(user.get(), order.get());
} // scope closes → any stragglers are cancelled
```

- `fork` → start a subtask on its own virtual thread.
- `join` → block until all subtasks finish (or one triggers shutdown).
- `get()` → read a result, only **after** a successful join.


## Shutdown Policies

| Policy | Behavior |
|---|---|
| `ShutdownOnFailure` | First failure cancels the rest — *all must succeed* |
| `ShutdownOnSuccess` | First success cancels the rest — *race for any winner* |

```java
// Race two sources, take whoever answers first
try (var scope = new StructuredTaskScope.ShutdownOnSuccess<String>()) {
    scope.fork(() -> callPrimary());
    scope.fork(() -> callBackup());
    return scope.join().result();   // fastest result wins
}
```
