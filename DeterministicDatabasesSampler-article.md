A pair of articles giving a broad overview of deterministic databases, as well 
as their possible applications in industry. 

Related to [these notes](ProblemsWithACIDand2PhaseCommit-blogpost.md).

# An Overview of Deterministic Database Systems
[link](https://www.cs.umd.edu/~abadi/papers/abadi-cacm2018.pdf)

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


