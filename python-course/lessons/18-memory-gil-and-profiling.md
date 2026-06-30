# Lesson 18: Memory, GIL, and Profiling

Principal engineers don't guess about performance — they measure, understand the GIL, and know where Python's object model helps or hurts.

## Python Object Model (Brief)

- Everything is an object; references are pointers to heap allocations
- Integers, strings, lists — all have type headers and reference counts
- CPython uses reference counting + cyclic GC for containers

```python
import sys
sys.getsizeof([])        # empty list overhead
sys.getsizeof("hello")   # string overhead + chars
```

## The Global Interpreter Lock (GIL)

One mutex allows **one thread at a time** to execute Python bytecode in CPython.

| Workload | Threads help? |
|----------|---------------|
| CPU-bound Python code | **No** — use `multiprocessing` |
| I/O-bound (network, disk) | **Yes** — threads release GIL during I/O |
| C extensions releasing GIL | **Yes** — NumPy, zlib, etc. |

The GIL simplifies memory management; it is why asyncio exists for high-concurrency I/O.

## __slots__ and __dict__

```python
class Slotted:
    __slots__ = ("x", "y")

class Dicted:
    def __init__(self, x, y):
        self.x, self.y = x, y
```

Use `__slots__` for millions of homogeneous records; avoid when flexibility matters.

## weakref — Caches Without Leaks

```python
import weakref

cache = weakref.WeakValueDictionary()

def get_expensive(key):
    obj = cache.get(key)
    if obj is None:
        obj = compute(key)
        cache[key] = obj
    return obj
```

Objects are evicted when no strong references remain.

## Profiling — cProfile

```bash
python -m cProfile -s cumtime script.py
```

In code:

```python
import cProfile
import pstats

with cProfile.Profile() as pr:
    run_workload()
stats = pstats.Stats(pr)
stats.sort_stats("cumtime").print_stats(20)
```

Focus on **cumulative time** for call chains; **tottime** for hot functions.

## line_profiler and memory_profiler

```bash
pip install line_profiler memory_profiler
kernprof -l -v script.py
python -m memory_profiler script.py
```

Use for targeted optimization after cProfile identifies hotspots.

## Allocation Tracking

```python
import tracemalloc

tracemalloc.start()
# ... work ...
snapshot = tracemalloc.take_snapshot()
for stat in snapshot.statistics("lineno")[:10]:
    print(stat)
```

## Optimization Hierarchy

1. **Algorithm** — O(n²) → O(n log n) beats micro-optimizations
2. **I/O** — batch requests, reduce round trips
3. **Libraries** — vectorized NumPy/pandas vs pure Python loops
4. **Concurrency model** — right tool for CPU vs I/O
5. **Micro-opts** — `__slots__`, local variable caching — last resort

## Key Takeaways

- CPython's GIL limits CPU parallelism in threads; plan accordingly.
- Profile before optimizing; cProfile first, line_profiler second.
- Memory tools (`tracemalloc`, `__slots__`, weakref) matter at scale.
- Principal engineers optimize bottlenecks, not everything.

## Exercises

1. Profile a naive Fibonacci implementation; compare with `@lru_cache`.
2. Benchmark threading vs multiprocessing for CPU-bound vs I/O-bound tasks.
3. Measure memory of 100k dataclass instances vs `__slots__` equivalents.
4. Document a performance investigation using before/after profiles.

## Solutions

See [exercises/18-memory-gil-and-profiling.md](../exercises/18-memory-gil-and-profiling.md).

## Next

[Lesson 19: Threading & Multiprocessing →](19-threading-and-multiprocessing.md)
