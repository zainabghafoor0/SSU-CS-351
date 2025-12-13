# Project-2: Threading and Multi-core Applications
CS 351 – Computer Architecture

Zainab Ghafoor

This project implements multi-threaded versions of two programs (**mean.cpp** and **sdf.cpp**) to explore parallel computation and performance scaling on multi-core CPUs. It includes timing analysis, speedup graphs, bandwidth calculations, and an Amdahl’s Law evaluation.

---

## Computing a Mean

### Speedup Graph
![Speedup graph for computing mean](mean_speedup.png)

---

### 1. Does the speedup curve converge?

Yes, the speedup curve converges. The curve rises up to about 14–16 threads, then begins to flatten around **55×** speedup. This flattening indicates the serial portion of the program is becoming the bottleneck. After ~16 threads, adding more threads yields almost zero improvement.

---

### 2. Maximum speedup achieved

The maximum speedup observed was **≈55.3× at 18 threads**.

---

### 3. What happens when using more threads than hardware cores?

When the thread count exceeds the number of available physical cores:

- Performance stops improving  
- Speedup may slightly worsen  

This is visible in the graph after 16 threads.

---

### 4. Do we get linear scaling with more cores?

No. The scaling is **sublinear**. From 16 to 18 threads the gain is almost zero, showing diminishing returns.

---

### 5. Amdahl’s Law: Estimate of p


Looking at the speedup graph for the mean computation, the curve rises quickly
and then flattens out around a maximum speedup of about **55×**. This plateau
is the practical maximum speedup, which we can use with Amdahl’s Law to
estimate `p`, the parallel fraction of the program.

Amdahl’s Law is:


    S(n) = 1 / ((1 - p) + p/n)

To get the *maximum* possible speedup, we imagine having an unlimited number of
threads. That means to take the limit as `n → infinity`.

When `n` becomes extremely large, the parallel term goes to zero:


    p/n → 0

So Amdahl’s formula becomes:

    S_max = 1 / ((1 - p) + 0)

Which simplifies to:

    S_max = 1 / (1 - p)

From the graph, I take:

    S_max ≈ 55

    55 = 1 / (1 - p)

    1 - p = 1 / 55

Now solve for `p`:

    p = 1 - 1/55
      ≈ 0.98182

So:

    p ≈ 0.98


This means that about **98% of the program is parallelizable** and about **2% is
inherently serial** (file I/O, setup, final reduction, etc.). That small serial
portion is what causes the speedup curve to flatten even when more threads are
added.
 

---

### 6. How many bytes of data are processed per iteration?

Each iteration reads a single `float`, so:

- **4 bytes per iteration**

---

### 7. Bandwidth estimation

Total samples:
N = 8,500,000,000

Total bytes:
N x 4 = 8.5 × 10^9 × 4 = 34 GB

Fastest run time:
1.33 s

Bandwidth:
34 GB / 1.33 s ≈ 25.6 GB/s


---

### 8. Is the bandwidth consistent across threaded versions?

No, bandwidth increases with thread count:

| Version                | Time (s) | Bandwidth |
|------------------------|----------|-----------|
| Serial                 | 73.01    | 0.47 GB/s |
| Threaded (1 thread)    | 10.87    | 3.1 GB/s  |
| Threaded (16 threads)  | 1.34     | 25.4 GB/s |
| Threaded (18 threads)  | 1.33     | 25.6 GB/s |


Bandwidth plateaus around **16+ threads**, matching the speedup flattening.

---

## Computing a Volume (SDF)

### Speedup Graph
![Speedup graph for computing volume](sdf_speedup.png)

---

### 9. Do you get a similar performance curve to threaded.out?

**Yes**, with some differences.

**Similarities:**
- Speedup rises quickly as thread count increases.
- Curve flattens once physical cores are saturated.
- Diminishing returns appear, as predicted by Amdahl’s Law.

**Differences:**
- Maximum speedup is lower.
- `sdf.cpp` is **compute-bound**, not memory-bound.
- It performs:
  - Random number generation  
  - Signed-distance calculations  
These operations limit parallel scaling compared to the bandwidth-bound mean computation.

---

