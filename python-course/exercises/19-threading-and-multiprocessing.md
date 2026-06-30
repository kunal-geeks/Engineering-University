# Exercise Solutions — Lesson 19

## 1. Thread pool downloader

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import httpx

def download_all(urls, workers=10):
    results = {}
    with ThreadPoolExecutor(max_workers=workers) as pool:
        futures = {pool.submit(httpx.get, u): u for u in urls}
        for f in as_completed(futures):
            url = futures[f]
            results[url] = f.result().status_code
    return results
```

## 3. Race demo + fix

```python
# Broken
counter = 0
def bad():
    global counter
    for _ in range(100_000):
        counter += 1

# Fixed — use Lock or multiprocessing.Value with lock
```

## 4. Decision doc outline

Compare request concurrency, blocking libraries, team familiarity, observability. Recommend asyncio for new async-native stack with high fan-out I/O; threads if mostly blocking legacy SDKs.
