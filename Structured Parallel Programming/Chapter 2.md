The best overall strategy for scalable parallelism is data parallelism

The opposite of data parallelism is functional decomposition, an approach that runs different program functions in parallel.

At best, functional decomposition improves performance by a constant
factor. For example, if a program has functions f , g, and h, running them in parallel at best triples performance, but only if all three functions take exactly the same amount of time to execute and do not depend on each other, and there is no overhead. Otherwise, the improvement will be less.

**A hardware thread** is a hardware entity capable of independently executing a program (a flow of instructions with data-dependent control flow) by itself. In particular it has its own “instruction pointer” or “program counter.” Depending on the hardware, a core may have one or multiple hardware threads. 
A **software thread** is a virtual hardware thread. An operating system typically enables many more software threads to exist than there are actual hardware threads by mapping software threads to hardware threads as necessary. A computation that employs multiple threads in parallel is called thread parallel.

**Vector parallelism** refers to single operations replicated over collections of data. In mainstream processors, this is done by vector instructions that act on vector registers. Each vector register holds
a small array of elements. For example, in the Intel Advanced Vector Extensions (Intel AVX) each register can hold eight single-precision (32 bit) floating point values. On supercomputers, the vectors may be much longer, and may involve streaming data to and from memory.

***With masking,*** a vector unit executes both arms of the original if statement but keeps only one of the results. 
**A thread** executes only the arm of interest.

**Packing** is an alternative implementation approach that rearranges fibers so that those in the same vector have similar control flow.

The process of compiling code to vector instructions is called **vectorization**


**In summary, threads and vectors are two hardware features for parallel execution. Threads deal with all kinds of parallelism but pay the cost of replicating control-flow hardware whether the replication is needed or not. Vectors are more efficient at regular computations when suitable vector instructions exist but can emulate irregular computations with some limitations and inefficiencies. In the best case, especially for large-scale regular computations, careful design can combine these mechanisms multiplicatively.**

Work-Span Model
**WORK**: It is the time that a serialization of the algorithm would take and is simply
the total time it would take to complete all tasks.
**SPAN**: The span is the time a parallel algorithm would take on an ideal machine with an infinite number of processors.
Span is equivalent to the length of the critical path. The **critical path** is the longest chain of tasks that must be executed one after each other. Synonyms for span in the literature are **step complexity or depth.**


**Asymptotic complexity** is the key to comparing algorithms. Comparing absolute times is not particularly meaningful, because they are specific to particular hardware. Asymptotic complexity reveals deeper mathematical truths about algorithms that are independent of hardware.

**In asymptotic analysis of serial programs, “O” is most common, because the usual intent is to prove an upper bound on a program’s time or space. For parallel programs, “Theta” is often more useful, because you often need to prove that a ratio, such as a speedup, is above a lower bound, and this requires computing a lower bound on the numerator and an upper bound on the denominator.** 
**For example, you might need to prove that using P workers makes a parallel algorithm run at least P times faster than the serial version. That is, you want a lower bound (Omega) on the speedup. That requires proving a lower bound on the serial time and an upper bound (“O”) on the parallel time.** 
**When computing speedup, the parallel time appears in the denominator and the serial time appears in the numerator. A larger parallel time reduces speedup while a larger serial time increases speedup. However, instead of dealing with separate bounds like this for each measure of interest, it is often easier to deal with the theta bound.**


