Structured serial programming is based on four control flow patterns: **sequence, selection, iteration, and recursion**

**A sequence** is a ordered list of tasks that are executed in a specific order. Each task is completed before the one after it starts.

A basic assumption of the sequence pattern is that the program text ordering will
be followed, even if there are no data dependencies between the tasks, so that side effects of the tasks such as output will also be ordered.

In the **selection pattern,** a condition c is first evaluated. If the condition is true, then some task a is executed. If the condition is false, then task b is executed. 
There is a control-flow dependency between the condition and the tasks so neither task a nor b is executed before the condition has been evaluated.
Also, exactly one of a or b will be executed, never both; this is another fundamental assumption of the serial selection pattern

There is a parallel generalization of selection, the **speculative selection pattern,**
In **speculative selection** all of a, b, and c may be executed in parallel, but the results
of one of a or b are **discarded** based on the result of computing c.

In the **iteration pattern**, a condition c is evaluated. If it is true, a task a is evaluated, then the condition c is evaluated again, and the process repeats until the condition becomes false.

One complication with parallelizing the iteration pattern is that the body task f may also depend on previous invocations of itself. These are called **loop-carried dependencies.**

Several parallel patterns can be considered parallelizations of specific forms of loops include **map, reduction, scan, recurrence, scatter, gather, and pack**. These correspond to different forms of loop  dependencies. 
You should be aware that there are some forms of loop dependencies that cannot be parallelized. One of the biggest challenges of parallelizing algorithms is that a single serial construct, iteration, actually maps onto many different kinds of parallelization strategies. Also, since data dependencies are not as important in serial programming as in parallel programming, they can be hidden.

**Recursion** is a dynamic form of nesting which allows functions to call themselves, directly or indirectly. It is usually associated with stack-based memory allocation or, if higher-order functions are supported, **closures** which are objects allocated on the heap.
**Tail recursion** is a special form of recursion that can be converted into iteration, a fact that is important in functional languages which often do not support iteration directly.



PARALLEL CONTROL PATTERNS
FORK-JOIN
The **fork–join pattern** lets control flow fork into multiple parallel flows that rejoin later. Various parallel frameworks abstract fork–join in different ways. Some treat fork–join as a parallel form of a compound statement; instead of executing substatements one after the other, they are executed in parallel.

Fork–join should not be confused with **barriers**. A barrier is a **synchronization** construct across multiple threads. In a barrier, each thread must wait for all other threads to reach the barrier before any of them leave. The difference is that after a barrier all threads continue, but after a join only one does. Sometimes barriers are used to imitate joins, by making all threads execute identical code after the barrier, until the next conceptual fork.

MAP
The **map pattern** replicates a function over every element of an index set. The
index set may be abstract or associated with the elements of a collection. The function being replicated is called an **elemental function** since it applies to the elements of an actual collection of input data.
The map pattern replaces one specific usage of iteration in serial programs: a loop in which every iteration is independent, in which the number of iterations is known is advance, and in which every computation depends only on the iteration count and data read using the iteration count as an index into a collection.

STENCIL
The **stencil pattern** is a generalization of the map pattern in which an elemental function can access not only a single element in an input collection but also a set of “neighbors.”

REDUCTION
A **reduction** combines every element in a collection into a single element using an associative **combiner function**.

SCAN
**Scan** computes all partial reductions of a collection. In other words, for every output position, a reduction of the input up to that point is computed.

RECURRENCE
The map pattern results when we parallelize a loop where the loop bodies are all independent. A recurrence is also a generalization of iteration, but of the more complex case where loop iterations can depend on one another. We consider only simple recurrences where the offsets between elements are constant. In this case, recurrences look somewhat like stencils, but where the neighbor accesses can
be to both inputs and outputs. 
A recurrence is like a map but where elements can use the outputs of adjacent elements as inputs.