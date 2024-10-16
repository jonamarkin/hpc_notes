The “free lunch” of automatically faster serial applications through faster microprocessors
has ended. The new “free lunch” requires scalable parallel programming. **The good news is that if you**
**design a program for scalable parallelism, it will continue to scale as processors with more parallelism**
**become available.**

There is  a mandatory parallelism and optional parallelism.
**Mandatory parallelism** forces the system to execute operations in parallel but may lead to poor
performance—for example, in the case of a recursive program generating an exponential number of
threads. Mandatory parallelism also does not allow for hierarchical composition of parallel software
components, which has a similar problem as recursion.


Patterns
Patterns can be loosely defined as commonly recurring strategies for dealing with particular
problems.
- Algorithm strategy patterns or algorithmic skeletons
	- Semantics: The semantics describe how the pattern is used as a building block of an algorithm, and consists of a certain arrangement of tasks and data dependencies
	- Implementations: 

The key point is that
different implementation choices may lead to different performances, but not to different semantics.
This separation makes it possible to reason about the high-level algorithm design and the low-level
(and often machine-specific) details separately.

