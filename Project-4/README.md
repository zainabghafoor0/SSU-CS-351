## Questions

### 1) Are the results what you expected?

Yes, the CPU implementation performed much faster than the CUDA implementation for small and medium vector sizes. Even for large vectors, the GPU was not significantly faster and was often slower.

### 2) Speculate as to why it looks like CUDA isn’t a great solution for this problem.

-CUDA introduces large overhead for memory transfers and kernel launches.  
-The `iota` function is too simple; it performs almost no computation relative to the amount of data being moved.  
-GPUs are only effective when the computational work is heavy enough to amortize memory transfer costs, which is not the case here.  
-CPUs are extremely fast at sequential memory writes.

CUDA provides no benefit here because the kernel does very little work, and the overhead dominates the runtime.

