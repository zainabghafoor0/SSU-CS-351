Which program is fastest? Always?
Across all tests, alloca.out was consistently the fastest for small and medium workloads. It averaged about 0.021s at 100k blocks and 0.195s at  1M blocks, edging out malloc.out. However, when the data per node was large (2–4 KB), all four programs performed almost the same. So alloca.out is generally the fastest, but not always because the advantage fades as node size increases.

Which program is slowest? Always?
list.out and new.out were the slower pair in every test, though which one was actually last varied slightly. new.out tended to be slowest for small lists, while list.out often took the longest on larger lists. When node sizes became large, all four implementations converged in runtime, so no clear slowest remained.

Trend vs size of data per Node?
Yes, as the data per node increased from small 10 bytes to large kilobytes, the total runtime increased for all programs. At the same time, the difference between implementations shrank.
With small nodes, allocator choice made a big difference; with large nodes, hashing and memory bandwidth dominated.

Trend vs chain length?
Yes, when increasing NUM_BLOCKS from 10k to 100k to 1M led to nearly linear growth in runtime. Programs with heavier per-node management (list.out, new.out) scaled worse.
alloca.out and malloc.out scaled best, staying consistently faster as the chain grew longer.

Heap breaks observations + stack vs heap?
alloca.out showed the same small number of breaks (69) regardless of list length. This is proof that it uses the stack. The heap based programs (malloc, new, list) all showed rising break counts as NUM_BLOCKS increased, since they had to expand the heap to satisfy new allocations. Changing the stack limit (ulimit -s unlimited) had no effect on heap activity because the stack and heap are separate memory regions.

Diagram requirement (two nodes, pointers, bytes[6])

flowchart LR
  H[head] --> N1
  N1[Node #1<br/>fields: next →, bytes* →, size=6] --> N2
  N2[Node #2<br/>fields: next →(null), bytes* →, size=6]
  T[tail] --> N2

  subgraph Storage
    B1[(6 bytes for Node #1)]
    B2[(6 bytes for Node #2)]
  end

  N1 -- bytes* --> B1
  N2 -- bytes* --> B2

 in alloca.cpp the nodes live on the stack as placement new; in malloc.cpp/new.cpp they live on the heap.


Which tasks were the same vs different across programs?
Data generation and hashing process are the same.
The difference is the memory acquisition method, such as std::list allocator vs new vs malloc+placement vs alloca+placement, and how each implementation maintains its linked list.

As data size increases, does node allocation matter more or less?
It becomes less. When nodes are tiny, the cost of frequent allocations dominates runtime differences. As each node holds more data, the hashing and copying work outweighs allocation overhead, so performance differences between allocation methods largely disappear.

