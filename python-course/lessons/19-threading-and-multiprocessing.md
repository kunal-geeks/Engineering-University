# Lesson 19: Threading and Multiprocessing

Choosing the wrong concurrency model wastes hardware and creates subtle bugs. Senior engineers map workload types to the right primitive.

## Decision Matrix

| Model | Best for | Avoid for |
|-------|----------|-----------|
| **Sequential** | Simple scripts, low load | High fan-out I/O |
| **threading** | Blocking I/O, legacy APIs | CPU-bound Python |
| **multiprocessing** | CPU-bound pure Python | Shared mutable state |
| **asyncio** | High-concurrency I/O, structured async | Blocking libraries without wrappers |
| **C extensions / Rust** | Hot CPU paths | Quick prototypes |

## threading — Shared Memory, GIL-Limited CPU

```python
import threading
from concurrent.futures import ThreadPoolExecutor, as_completed

def fetch(url):
    import httpx
    return httpx.get(url).status_code

urls = ["https://example.com"] * 20

with ThreadPoolExecutor(max_workers=10) as pool:
    futures = [pool.submit(fetch, u) for u in urls]
    for f in as_completed(futures):
        print(f.result())
```

### Locks and Races

```python
counter = 0
lock = threading.Lock()

def increment():
    global counter
    with lock:
        counter += 1
```

Without the lock, `counter += 1` is not atomic (read-modify-write).

Prefer `queue.Queue` over shared lists for producer-consumer patterns.

## multiprocessing — Separate Interpreters

```python
from concurrent.futures import ProcessPoolExecutor

def cpu_heavy(n):
    return sum(i * i for i in range(n))

with ProcessPoolExecutor() as pool:
    results = pool.map(cpu_heavy, [10**6] * 8)
```

Each process has its own GIL — true CPU parallelism.

### IPC Costs

- Pickling arguments and results
- Higher memory footprint
- Use for coarse-grained tasks (seconds of CPU), not tiny functions

## multiprocessing.Queue vs threading.Queue

Process queues serialize data; thread queues pass object references.

## fork vs spawn (macOS/Windows)

Default start method on macOS is `spawn` — imports re-run in child processes. Guard entry points:

```python
if __name__ == "__main__":
    main()
```

## concurrent.futures — Unified API

```python
from concurrent.futures import wait, FIRST_COMPLETED

done, pending = wait(futures, return_when=FIRST_COMPLETED)
```

## Common Pitfalls

- Deadlocks from lock ordering
- Shared mutable globals in multiprocessing (each process gets a copy at fork/spawn)
- Daemon threads killed abruptly on shutdown
- Thread pools sized wrong (often `min(32, cpu+4)` for I/O)

## Key Takeaways

- Threads for blocking I/O; processes for CPU-bound Python.
- Always synchronize shared mutable state or use message passing.
- `concurrent.futures` is the high-level API of choice.
- Measure — wrong model can be slower than sequential.

## Exercises

1. Build a thread pool downloader; compare throughput vs sequential.
2. Parallelize a CPU-bound transform with `ProcessPoolExecutor`; report speedup on N cores.
3. Demonstrate a race condition without locks; fix with `Lock` or `Queue`.
4. Write a decision doc: threading vs asyncio for your team's HTTP service.

## Solutions

See [exercises/19-threading-and-multiprocessing.md](../exercises/19-threading-and-multiprocessing.md).

## Next

[Lesson 20: Asyncio Deep Dive →](20-asyncio-deep-dive.md)
