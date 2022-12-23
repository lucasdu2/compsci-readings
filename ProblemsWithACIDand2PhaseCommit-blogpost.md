Two blog posts by Daniel Abadi on the problems with ACID and why we should
move on from Two Phase Commit. These are very much related discussions on some
venerable database-related conventions (ACID properties, Two Phase Commit for
transactions) and their shortcomings. Although they were written nearly 10 years
apart, they represent different facets of a major throughline in Abadi's research:
deterministic database systems, and more broadly, a desire for more performant
distributed protocols. 

# It’s Time to Move on from Two Phase Commit
[link](http://dbmsmusings.blogspot.com/2019/01/its-time-to-move-on-from-two-phase.html)

[internet archive link](https://web.archive.org/web/20220826233144/http://dbmsmusings.blogspot.com/2019/01/its-time-to-move-on-from-two-phase.html)

## Background Knowledge
- Two-phase commit (2PC) is a well established protocol that ensures atomicity
and durability of transactions
- Used widely in industry to support ACID guarantees for transactions on sharded/
partitioned databases
- Some quick notes on how 2PC works (this is already well-documented online in 
other places):
    - A commit manager/coordinator and a set of workers
    - Each worker has an independent task to work on, all of the workers together
will do all the work specified in the transaction
    - 2PC is a protocol that handles the atomicity and durability of a transaction
in the face of fail-stop errors
    - Phase 1: Coordinator asks worker if they have completed their work, workers
will respond "yes" or "no"
    - Phase 2: Once coordinator determines that every worker has responded "yes",
coordinator sends a "commit" message to each worker. Once a workers receives a 
"commit" message, it will commit its work and return success to the coordinator.
- 2PC has lots of problems, particularly with performance
- 2 major problems, one well-known, one less well-known (according to Abadi)
    1. Well-known problem = the "blocking problem." Basically, when each worker
has voted "yes," but the coordinator fails before sending a commit decision. Then
all workers have to wait for the coordinator to come back up; it's a single point
of failure. Fundamental reason for this problem is that once workers return "yes,"
they essentially give up all power to veto a transaction to the coordinator. 
    2. Less-well-known problem = the "cloggage problem." Worker nodes do not know
until part-way through Phase 2 of the protocol if the transaction will be committed.
Until they know the final outcome, they have to block other conflicting transactions
(transactions that may operate on the same data) until the transaction is 
guaranteed to commit. Blocked transactions can, in turn, block other transactions,
etc. which could cause a traffic jam. 
- Below is a quote from the post that does a good job summarizing 2PC's problems:
> o summarize the problems we discussed above: 2PC poisons a system along four 
dimensions: latency (the time of the protocol plus the stall time of conflicting 
transactions), throughput (because it prevents conflicting transactions from 
running during the protocol), scalability (the larger the system, the more 
likely transactions become multi-partition and have to pay the throughput and 
latency costs of 2PC), and availability (the blocking problem we discussed above).  
Nobody likes 2PC, but for decades, people have assumed that it is a necessary evil.

## Novel Ideas that Address 2PC Issues
- Abadi believes we can do away to 2PC by interrogating a fundamental assumption
built into contemporary distributed system architecture: that **a transaction can
abort at any time, for any reason**
- The arbitrary potential for a transaction to abort is the reason for much of
the complexity/latency in the protocol
- Below quote does a good job summarizing Abadi's point:
> In my opinion we need to remove veto power from workers and architect systems 
in which the system does not have freedom to abort a transaction whenever it wants 
during its execution. Only logic within a transaction should be allowed to cause 
a transaction to abort.
- Essentially, system-level problems should *not* cause aborts, all aborts should
be known once the transaction finishes processing (which is before the 2PC
protocol kicks in, 2PC only actually becomes relevant once a worker has completed
its particular work in the transaction)
    - Abadi calls these transaction logic-related aborts "state-induced aborts"
- In fact, not allowing system-level aborts does away entirely with the need
for a commit protocol (the entire point of the commit protocol is to ensure
that a system-level failure does not cause data inconsistency)
- If we only allow state-induced aborts, once the transaction is finished on 
each worker we can guarantee that the transaction will commit--there is no further
need for a complex commit protocol

## Wrap-Up
- Fundamental assumption that arbitrary system-level aborts can always happen
feels very natural (at least to me), so Abadi's challenging of this assumption
is very interesting
- Simply not allowing system-level aborts does greatly simplify the design space
for distributed transactions, since it does away with a lot of the cross-partition
communication needed to handle them
- But this obviously isn't a free win, since system-level failures can clearly
still happen, even if we choose not abort when they happen; we have to handle them 
in some other way
- It seems to me that we will need to store more data on each worker in order to
generate a working base from which we can recover from failure--this comes with
its own set of challenges that are not explicity addressed in the post

# The problems with ACID, and how to fix them without going NoSQL
[link](http://dbmsmusings.blogspot.com/2010/08/problems-with-acid-and-how-to-fix-them.html)

[internet archive link](https://web.archive.org/web/20221206092937/http://dbmsmusings.blogspot.com/2010/08/problems-with-acid-and-how-to-fix-them.html)

## Background
- According to Abadi, much of the impetus to adopt NoSQL comes from the limitations
ACID itself puts on scalability
- NoSQL systems are not really about eliminating SQL, but rather about increasing
scalability on commodity-hardware, shared-nothing cloud architectures (which 
are quite common at the moment)
    - It does this by loosening ACID requirements
    - Thus, in Abadi's view, "NoSQL really means NoACID"
- Generally, guaranteeing ACID properties for transactions that operate across
multiple machines is very complicated and difficult
    - Atomicity requires a distributed commit protocol
    - Isolation requires that the transaction hold all its locks for the duration
of the protocol
    - Additionally, the need for high availability often means we have to use
strongly consistent replication schemes, which are expensive
> In summary, it is really hard to guarantee ACID across scalable, highly 
available, shared-nothing systems due to complex and high overhead commit 
protocols, and difficult tradeoffs in available replication schemes.
- NoSQL solutions loosen ACID guarantees to achieve higher performance
- It is the view of Abadi and his collaborators that this is the "lazy solution"
    - Essentially, the problems of ensuring ACID properties gets shifted from
the underlying database to the application itself (and by extension, the
programmers, who have to deal with the resulting implementation overhead)

## Novel Ideas
- Instead of relaxing ACID, Abadi wants a way for fully ACID systems to be able to 
scale on shared-nothing architectures
- The core insight is this: **"...the problem with ACID is not that its guarantees 
are too strong (and that therefore scaling these guarantees in a shared-nothing 
cluster of machines is too hard), but rather that its guarantees are too weak, 
and that this weakness is hindering scalability."**
- Somewhat counterintuitively, the premise is that *stricter consistency guarantees
will not lead to less performance, but will actually INCREASE performance, at least
in cheap, shared-nothing distributed architectures*
- 

