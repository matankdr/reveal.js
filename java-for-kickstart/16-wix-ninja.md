# Concurrency at Wix Ninja


## Concurrency at Wix Ninja

- **Managed Executors** — NEVER create your own thread pools
- **Context Propagation** — Ninja executors automatically propagate Trace IDs for logging
- **Observability** — built-in integration with Grafana for monitoring thread health

```java
// Standard Wix usage
var executor = context.executorService();

executor.submit(() -> {
    // Trace ID is preserved here
    someWork();
});
```
