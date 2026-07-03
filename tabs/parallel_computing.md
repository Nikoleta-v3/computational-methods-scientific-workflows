---
layout: workshop
title: Parallel Computing Tools
---

## 05: Parallel Computing Tools

In this section, we return to the `src/compute.py` example and parallelise it
with Dask.

The aim is not to learn every parallel computing tool. The aim is to understand
the basic idea: split one large computation into smaller independent pieces,
run those pieces at the same time, and combine the partial results.

We will look at two Python versions:

- using Dask with threads on one machine
- using Dask distributed, which can also run across multiple workers or nodes

For the C++ part, we will not write the full code in this workshop page. We
will point to an online repository where the C++ versions live.

## Starting point: the serial script

The current `src/compute.py` script computes the sum of integers from `0` to
`N - 1`:

```python
import sys
import time

if __name__ == "__main__":
    N = 1_000_000_000
    if len(sys.argv) > 1:
        N = int(sys.argv[1])

    total = 0

    start = time.perf_counter()

    for i in range(N):
        total += i

    end = time.perf_counter()

    print(f"Sum: {total}")
    print(f"Time: {end - start:.6f} s")
```

Run a small version first:

```shell
python src/compute.py 100000000
```

or:

```shell
python3 src/compute.py 100000000
```

This is a deliberately simple example. In real research code, the work inside
the loop might be a simulation, an analysis step, or a calculation on a subset
of the data.

## The parallelisation idea

Instead of summing the full range in one loop, we can split the range into
chunks.

For example, if `N = 100`, we could split the work into four chunks:

```text
0-24
25-49
50-74
75-99
```

Each chunk can be summed independently. After all chunks are finished, we add
the partial sums together.

This is the pattern:

1. split the work
2. run the chunks in parallel
3. combine the results

## Install Dask

If Dask is not already installed, install it:

```shell
pip install dask distributed
```

or:

```shell
pip3 install dask distributed
```

## Version 1: Dask with threads

Create a new file:

```shell
touch src/compute_dask_threads.py
```

Add the following code:

```python
import math
import sys
import time

import dask


def chunk_sum(start, stop):
    total = 0
    for i in range(start, stop):
        total += i
    return total


if __name__ == "__main__":
    N = 1_000_000_000
    if len(sys.argv) > 1:
        N = int(sys.argv[1])

    n_workers = 4
    if len(sys.argv) > 2:
        n_workers = int(sys.argv[2])

    chunk_size = math.ceil(N / n_workers)

    start_time = time.perf_counter()

    tasks = [
        dask.delayed(chunk_sum)(
            start,
            min(start + chunk_size, N),
        )
        for start in range(0, N, chunk_size)
    ]

    partial_sums = dask.compute(
        *tasks,
        scheduler="threads",
        num_workers=n_workers,
    )

    total = sum(partial_sums)

    end_time = time.perf_counter()

    print(f"Workers: {n_workers}")
    print(f"Sum: {total}")
    print(f"Time: {end_time - start_time:.6f} s")
```

Run it:

```shell
python src/compute_dask_threads.py 100000000 2
```


The first number is `N`. The second number is the number of workers.

The answer should match the serial version:

```shell
Sum: 4999999950000000
```

The runtime may or may not improve for this example. This computation is very
simple, so the overhead of creating tasks can be larger than the benefit of
parallelism. That is an important lesson: parallel code is not automatically
faster.

### Save the threaded version with git

Once the threaded version runs and gives the same answer as the serial script,
check the repository status:

```shell
git status
```

Add the new script:

```shell
git add src/compute_dask_threads.py
```

Inspect what is staged:

```shell
git diff --staged
```

Commit it:

```shell
git commit -m "Add threaded Dask compute example"
```

## Version 2: Dask distributed

Dask distributed uses a scheduler and workers. The scheduler coordinates the
work, and the workers run the tasks.

On your laptop, this can still run locally. On a cluster, the workers can run on
different nodes.

Create a new file:

```shell
touch src/compute_dask_distributed.py
```

Add the following code:

```python
import math
import sys
import time

from dask.distributed import Client, LocalCluster


def chunk_sum(start, stop):
    total = 0
    for i in range(start, stop):
        total += i
    return total


if __name__ == "__main__":
    N = 1_000_000_000

    n_nodes = 1
    if len(sys.argv) > 1:
        n_nodes = int(sys.argv[1])

    workers_per_node = 4
    if len(sys.argv) > 2:
        workers_per_node = int(sys.argv[2])

    total_workers = n_nodes * workers_per_node
    chunk_size = math.ceil(N / total_workers)

    cluster = LocalCluster(
        n_workers=total_workers,
        threads_per_worker=1,
        processes=True,
        dashboard_address=None,
    )

    client = Client(cluster)

    start_time = time.perf_counter()

    futures = [
        client.submit(
            chunk_sum,
            start,
            min(start + chunk_size, N),
        )
        for start in range(0, N, chunk_size)
    ]

    partial_sums = client.gather(futures)
    total = sum(partial_sums)

    end_time = time.perf_counter()

    print(f"Nodes: {n_nodes}")
    print(f"Workers per node: {workers_per_node}")
    print(f"Total workers: {total_workers}")
    print(f"Sum: {total}")
    print(f"Time: {end_time - start_time:.6f} s")

    client.close()
    cluster.close()
```

Run it:

```shell
python src/compute_dask_distributed.py 1 4
```


This version starts a local Dask cluster. The same structure can also be used
with a Dask dashboard if you enable one when creating the cluster.

### Save the distributed version with git

Again, treat the working script as a checkpoint:

```shell
git status
git add src/compute_dask_distributed.py
git diff --staged
git commit -m "Add distributed Dask compute example"
```

## Running on multiple nodes

The code above uses `LocalCluster`, which starts workers on the same machine.
On a cluster or multiple-node system, the key change is how the Dask client is
created.

For example, if a Dask scheduler is already running somewhere, the script might
connect to it like this:

```python
from dask.distributed import Client

client = Client("tcp://SCHEDULER-ADDRESS:8786")
```

The rest of the code can stay very similar: create tasks, submit them, gather
the results, and combine them.

Different computing centres provide different ways to start Dask workers. Some
use job schedulers such as SLURM. Some use containers. Some provide a Dask
gateway. The important conceptual point is that Dask lets the same task-based
workflow run locally first and then move to a larger system.

## What to compare

When comparing the serial and parallel versions, check:

- whether the answer is the same
- whether the runtime improves


For this example, you may not see a speedup. The calculation is intentionally
simple. In real research code, parallelism is more useful when each chunk of
work is expensive enough to justify the overhead.

## C++ approaches

For C++, we will point to an online repository rather than writing all versions
in this lesson page.

Add the C++ repository link here: https://github.com/yohm/hello_openmp_mpi.

This was developed by Dr Yohsuke Murase, my group leader and an expert in
high-performance computing.

The C++ examples should demonstrate the same idea as the Python examples:

- start with a serial implementation
- split the work into chunks
- parallelise the chunks
- combine the partial results

Common C++ approaches include:

- OpenMP for shared-memory parallelism on one machine
- MPI for distributed-memory parallelism across multiple nodes

## Other Python tools worth mentioning

For this workshop, Dask is enough to demonstrate the main ideas. It works well
for task-based workflows, larger-than-memory arrays or data frames, and moving
from a laptop to a cluster-like setup.

Other tools are worth knowing about:

- `concurrent.futures`: useful for simple standard-library thread or process
  pools
- `multiprocessing`: useful for process-based parallelism in the Python
  standard library
- `joblib`: common in the scientific Python ecosystem, especially around
  scikit-learn-style workflows
- `mpi4py`: useful when working directly with MPI from Python
- Ray: useful for larger distributed Python applications
