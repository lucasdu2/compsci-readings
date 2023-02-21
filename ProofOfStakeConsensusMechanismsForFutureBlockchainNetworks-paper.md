# Proof-of-Stake Mechanisms for Future Blockchain Networks
[link](https://ieeexplore.ieee.org/document/8746079)
- Interesting overview of blockchain/distributed consensus, as well as an 
introduction to **proof-of-stake consensus mechanisms**, which hold a lot of 
promise in terms of making blockchain-based applications more energy-efficient 
and more time-efficient (among other benefits) compare to the current proof-of-work
based consensus that is widely used
- Good follow-up paper (at least for me) to the original Bitcoin paper, the 
Blockstack paper (an application of blockchain ideas to a PKI), and the original 
Ethereum whitepaper. These papers were included in the 2022 iteration of the 
old MIT 6.824 Distributed Systems course, which can currently be found online.
    - [Bitcoin paper]()
    - [Blockstack]()
    - [Ethereum whitepaper]()

## Overview of Blockchain Networks and Consensus Mechanisms
- In blockchain, transactions are stored in blocks, which are kept in a
continuously growing, cryptographically secured chain--this is basically a signed
log ala the one used in [SUNDR](https://www.usenix.org/legacy/event/osdi04/tech/full_papers/li_j/li_j.pdf)
- Transactions hold data, e.g. Alice sends Bob some money
- Upon receiving a transaction, a "miner" (a participant in the consensus 
mechanism) will try to validate and then include a transaction in a block
- The verification involves validating cryptographic hashes, which in theory
ensure that **the chain is tamper-evident** (if someone tries to forge a 
transaction, people will be able to tell)
- There will be many miners working on verifying transactions and creating the 
next block--the process of determining which miner is the one actually allowed
to add the official next block is very important
    - In the first iteration of blockchain-based applications (e.g. Bitcoin,
Namecoin) used a proof-of-work method to determine the "leader" miner
    - Basically, miners all had to solve a hard computational puzzle (e.g. find
a nonce that, when hashed with the previous block's header results in a hash
with a certain number of 0s at the front)
    - This puzzle ensured that being able to consistently become the leader would
take an exorbitant amount of computational (in Bitcoin, 51% of the total computational
power of the network)
    - In theory, this would be a strong deterrent to fraud, since it would take
immense computational power to commit fraudulent transactions to the chain
- Benefits:
    - Decentralization: No central controller, no trusted parties or single point
of failure 
    - Transparency: Data in blockchain visible to all
    - Immutability: Data in blockchain is hard to alter after-the-fact
    - Security, Privacy: Built-in cryptographic protocols means that, in theory,
if a user can keep their private key a secret, it is impossible to forge the user's
transactions and easy to verify the user's legitimate transactions


## Consensus Mechanism Categories
- Proof-of-Work
    - Used by early blockchain networks, involves a heavy computational problem
that must be solved in order for a miner to "win" the round and get permission
to add its block the chain
    - Essentially, **a weighted random coin-tossing process** where a particpant
with higher hash rate (computational power) have higher chances to win
- Proof-of-Concepts
    - Based on PoW framework, tries to make better use of the computational
problem in PoW by making it something useful (and not a "meaningless" hashing 
competition)
    - Puzzle could be a practical mathematical problem (e.g. Primecoin), or in
many instances, a proof of storage for distributed data storage (e.g. Permacoin,
Filecoin)
- Proof-of-Stake
    - Instead of selecting based on computational power (PoW), we select based
on the "stakes" a miner is holding (aka the number of coins/tokens in the 
blockchain system that the miner owns)
- Hybrid Consensus Mechanisms
    - Combination of PoW and PoS protocols, usually built from PoW systems but
adding a PoS component somewhere to lessen the computational requirements of the
protocol

## Proof-of-Stake Details and Analysis
Probably the most interesting part of the paper to me. 
