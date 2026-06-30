# Lesson 20: Asyncio Deep Dive

Asyncio powers modern Python web stacks (FastAPI, Starlette, aiohttp). Principal engineers understand event loops, task lifecycles, and failure modes — not just `async def`.

## Core Concepts

- **Coroutine** — `async def` function; must be awaited
- **Event loop** — schedules coroutines on I/O readiness
- **Task** — coroutine wrapped for concurrent execution
- **Future** — low-level placeholder for a result

```python
import asyncio

async def fetch_data(delay: float) -> str:
    await asyncio.sleep(delay)
    return "data"

async def main():
    results = await asyncio.gather(
        fetch_data(1),
        fetch_data(2),
        fetch_data(0.5),
    )
    print(results)

asyncio.run(main())
```

## Tasks and Cancellation

```python
async def main():
    task = asyncio.create_task(long_running())
    await asyncio.sleep(1)
    task.cancel()
    try:
        await task
    except asyncio.CancelledError:
        print("cancelled cleanly")
```

Always handle `CancelledError` in cleanup paths; re-raise after cleanup.

## Timeouts

```python
async def with_timeout():
    try:
        return await asyncio.wait_for(slow_call(), timeout=5.0)
    except asyncio.TimeoutError:
        return fallback()
```

## Semaphores and Rate Limiting

```python
sem = asyncio.Semaphore(10)

async def bounded_fetch(url):
    async with sem:
        return await client.get(url)
```

## Running Blocking Code

Never block the event loop:

```python
async def get_file_hash(path):
    return await asyncio.to_thread(hash_file, path)  # Python 3.9+
```

For CPU-heavy work, use `ProcessPoolExecutor` via `loop.run_in_executor`.

## Async Context Managers and Generators

```python
async with aiohttp.ClientSession() as session:
    async with session.get(url) as resp:
        body = await resp.text()

async for line in async_read_lines(path):
    process(line)
```

## Structured Concurrency (Python 3.11+)

```python
async def main():
    async with asyncio.TaskGroup() as tg:
        tg.create_task(job_a())
        tg.create_task(job_b())
    # all done or first exception cancels siblings
```

Prefer `TaskGroup` over bare `gather` for failure propagation.

## Streams

```python
async def handle(reader: asyncio.StreamReader, writer: asyncio.StreamWriter):
    data = await reader.read(100)
    writer.write(data.upper())
    await writer.drain()
    writer.close()
```

## Common Pitfalls

| Pitfall | Fix |
|---------|-----|
| Calling blocking DB driver in async route | Use async driver or `to_thread` |
| Fire-and-forget tasks | Keep references; use `TaskGroup` |
| Shared mutable state without locks | Use asyncio locks or immutable data |
| Nested event loops | One loop per thread; use `asyncio.run` |

## Async vs Threads

- **Asyncio**: single-threaded, low overhead, explicit `await` — best for 10k+ concurrent I/O connections
- **Threads**: blocking code works, higher memory — good for moderate I/O with legacy libs

## Key Takeaways

- Async shines for high-concurrency I/O; blocking the loop kills throughput.
- Use `gather`/`TaskGroup`, semaphores, and timeouts deliberately.
- Offload blocking/CPU work with `to_thread` or process pools.
- Cancellation and cleanup are part of the design, not afterthoughts.

## Exercises

1. Build an async URL fetcher with concurrency limit and per-request timeout.
2. Refactor blocking file I/O into `asyncio.to_thread` without blocking the loop.
3. Use `TaskGroup` where one failing task cancels peers; log cleanup.
4. Benchmark asyncio vs threads for 500 HTTP requests; explain results.

## Solutions

See [exercises/20-asyncio-deep-dive.md](../exercises/20-asyncio-deep-dive.md).

## Next

[Lesson 21: Testing Architecture →](21-testing-architecture.md)
