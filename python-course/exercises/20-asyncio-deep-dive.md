# Exercise Solutions — Lesson 20

## 1. Bounded async fetcher

```python
import asyncio
import httpx

async def fetch_all(urls, concurrency=10, timeout=5.0):
    sem = asyncio.Semaphore(concurrency)
    async with httpx.AsyncClient(timeout=timeout) as client:
        async def one(url):
            async with sem:
                return (url, (await client.get(url)).status_code)
        return await asyncio.gather(*[one(u) for u in urls])
```

## 2. asyncio.to_thread

```python
async def read_file_async(path):
    return await asyncio.to_thread(Path(path).read_text, encoding="utf-8")
```

## 3. TaskGroup with cleanup

```python
async def main():
    try:
        async with asyncio.TaskGroup() as tg:
            tg.create_task(job_a())
            tg.create_task(job_b())
    except* Exception as eg:
        log.exception("task failed", errors=eg.exceptions)
    finally:
        await cleanup_resources()
```
