A pair of articles giving a broad overview of deterministic databases, as well 
as their possible applications in industry. 

Related to [these notes](ProblemsWithACIDand2PhaseCommit-blogpost.md).

# An Overview of Deterministic Database Systems
**Authors: DANIEL J. ABADI, JOSE M. FALEIRO**
[link](https://www.cs.umd.edu/~abadi/papers/abadi-cacm2018.pdf)
- Summary article based on current research (as of 2018) on deterministic
databases; Abadi is one of the main drivers of this research and is the first
author on this article

## Basic Principles and Properties
- Main idea: given an initial state and a defined input to the system, two
independent machines must end up in one possible final state
- The primary principles/properties that make this possible:
    - **Input preprocessing:** Some component of the system must create a 
canonical record of input to the system. In non-deterministic systems, multiple
inputs can execute independently, in a non-deterministic way; this is not okay
in a deterministic system. Also, the preprocessing must replace inherently
non-deterministic code (e.g. get current time) with deterministic code.
    - **Non-deterministic failures do not cause transaction failure:** Non-deterministc
failures like server failure are not allowed to abort a transaction. This is a
major departure from "conventional" non-deterministic systems, which abort and 
retry on non-deterministic failure like server failure. Deterministic systems
can avoid aborting by recreating state at the point of non-deterministic failure.
    - **Thread race conditions cannot affect database state:** Concurrent
transactions usually run in separate threads/processes. These threads are 
scheduled in a fundamentally non-deterministic way by the OS, which can lead to
race conditions that are also non-deterministic. For example, when 2 transactions
simultaneously try to purchase the same item in a pessimistic, lock-based system,
the first thread to acquire the lock will succeed. 
    - **Deadlocks cannot occur in the system:** A deadlock is typically the
result of a non-deterministic race condition. If we reach a deadlock, we will
have to abort one of the deadlocked transactions. This is represents a
non-deterministic abort that cannot happen in a deterministic system. 
    - **Recovery from input log:** Recovery in deterministic systems is easier
because we can just load a checkpoint and play an input log. In non-deterministic
systems, we have to actually replay history (since the input itself does not
define behavior), which is more expensive.
    - **No reliance on OS for enforcing determinism:** Enforcing application/
database level determinism avoids overhead associated with enforcing determinism
at a lower level (like the OS or language runtime). This is primarily due to 
the fact that knowledge of the application allows for more optimizations, while
lower level enforcement must be stricter and thus often less performant.

## Implementation Techniques
The primary difficulty in implementing deterministic databases is to allow for 
concurrency while simultaneously ensuring determinism. Note that it is quite
easy to ensure determinism if we simply do everything in serial order, but this
is much less performant/scalable. Here are 3 approaches that support concurrent
transactions while still ensuring determinism.
1. Partitioning
    - Simplest approach: simply ensure that each thread is completely independent
from each other. For example, by partitioning data and using a single thread
to handle operations on each partition (H-Store)--this strategy breaks down when
a transaction needs to access data across multiple partitions.
2. Ordered locking
    - High level idea: locks must be requested in the order that transactions
appear in the input log
    - This will guarantee deadlock-freedom and equivalence to sequential 
execution of the input log
    - However, can lead to limited concurrency: in particular, if the data 
needed for a transaction is unknown at the beginning of the transaction, we 
cannot begin a subsequent transaction until the point where we can determine all
the locks the transaction needs
    - So basically if a transaction does not know its entire access-set in 
advance, we have to execute transactions almost entirely serially 
    - This method can be augmented by a technique called OLLP (this is 
discussed in detail in the [Calvin paper](https://cs.yale.edu/homes/thomson/publications/calvin-sigmod12.pdf)), which essentially runs the transaction in a trial mode
that determines the access-set ahead of time, before the transaction actually
runs
3. Dependency graphs
    - Most recent development: a dependency graph is generated from the input
log before transactions start executing
    - Nodes are transactions, edges correspond to conflicts between transactions,
direction of edges determines order of conflicting transactions in the input log
    - This technique avoids centralized processing/data structures during graph
construction and execution
    - Multi-versioning (multi-version concurrency control) can be used with 
this idea to even enable concurrent executions of conflicting transactions

## Advantages of Determinism
1. Replication
    - Replication is much easier than in non-deterministic databases
    - No need for a complex protocol to ensure that state is equivalent across
servers because a deterministic database already guarantees that as long as the 
same input log is shipped to each server, the end result will always be the same
2. Scalability
    - When scaling a database out to multiple servers, non-deterministic 
databases need a way to ensure atomicity of cross-partition transactions
    - This is usually done through a distributed commit protocol like 2-Phase
Commit, which guarantees atomicity/durability by making sure that each server
involved is up and prepared to commit
    - In a deterministic system, there is no longer a need for distributed 
commit protocols, since a transaction is simply not allowed to abort for a
non-deterministic reason like a failed node
    - If a node fails, the transaction will simply be delayed
    - More specifically, for a transaction that does not contain a deterministic
abort (e.g. some check in the code fails), there is no need at all for a commit
protocol of any kind. And for transactions with deterministic aborts, nodes
involved in those transactions can immediately vote "yes" once they are sure
they will not deterministically abort. So transactions will never need to wait
until the end of processing before starting the commit protocol.
3. Concurrency
    - Main advantage in concurrency is the way that deterministic systems enable
multi-version concurrency control (MVCC) to be applied to a greater extent than is 
possible in non-deterministic systems
    - General appeal of MVCC is that reads and writes to the same data can be
decoupled because we save multiple versions of data (new version for each write)
    - In deterministic systems, we create a global input log of all transactions 
and create a dependency graph from this log--this dependency graph contains
explicit information about read/write dependencies and orderings across 
transactions, which makes very fine-grained MVCC policies possible
    - One particularly interesting one: we can get better performance from
strong isolation by using the dependency graph to enable fine-grained decomposition
of a transaction. Traditionally, this sort of decomposition and interleaving of
different parts of different transactions was only possible in a system with
weak isolation guarantees, but the pre-calculation done by deterministic systems
allows decomposition approaches to still guarantee serializability, atomicity,
and recoverability. 
4. Logging overhead
    - Final state determined only by input log in deterministic systems, no
additional logging overhead required
    - Contrast to non-deterministic systems, which need to log all changes to 
database state as they happen and force these logs to stable storage in order
to guarantee data durability
5. System modularity
    - Database management systems are notoriously monolithic
    - One fundamental reason for this is the concurrency control protocols in
a non-deterministic system--not only non-deterministic, but system components
must often have knowledge of the internal state of the concurrency control module
in order to work with it (specifically for logging and recovery)
    - Deterministic systems have a "single source of truth" in the input log
    - This log is a declarative specification of concurrency control behavior
    - Creates a clean interface for other system components to look at
    - For example, with a dependency graph (which is created from the input log),
we can give transactions to execution threads that we already know will not 
conflict with currently running transactions
    - Thus, once we hand the execution over, there is no longer any need for the
execution thread to communicate with the concurrency control component of the
system

## Downsides to Determinism
1. Input preprocessing
    - Because we must preprocess all incoming transactions in order to 
support our determinism guarantees, latency will be increased
    - Additionally, the concurrency techniques described in this article all
require foreknowledge of transactions to be executed, which makes "interactive
transactions" more difficult

# Deterministic Databases and the Future of Data Sharing
**Article date: 09-29-2022**
[link](https://thenewstack.io/deterministic-databases-and-the-future-of-data-sharing/)
- Problem: sharing data across different clouds/regions/isolated systems
    - Companies that share data often want a "single source of truth"
    - Right now, not much has been done to address this problem
    - APIs offer a basic solution, but offer no guarantees (because they don't
understand the data, they cannot check if data is consistent/up-to-date/etc)
    - Blockchain as it currently stands is not performant and lacks a familiar
database-like interface
- Generally, article discusses the similarities/convergences of deterministic
databases and blockchain
    - "A blockchain is just a special kind of deterministic database"
- However, deterministic databases are scoped within a single "walled garden" 
data store; blockchain is a protocol that shares data across multiple, individual
data stores
- Given the intersections of the two fields, there is an opportunity to take the
existing research on deterministic databases and apply certain things to 
blockchain development
    - Goal would be to take the secure/zero-trust/decentralized nature of 
blockchain sharing and get some of the performance and consistency/availability
guarantees that deterministic databases provide
    - Would allow for more performant and transaction-based data sharing across
a secure blockchain
    - Article mentions [Vendia](https://www.vendia.com/product) as a company 
that is trying to do this ([Internet Archive link](https://web.archive.org/web/20221206193934/https://www.vendia.com/product))


