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
### Basic Fundamentals
- Proof-of-Stake as an alternative to Proof-of-Work
    - Instead of computational power resources, leaders selected based on stakes
in the network (contributions to the blockchain network, stake of a node is the
number of digital tokens it holds/deposits)
    - Proof-of-Work: consume tons of energy working on computational problem
to try to become leader
    - Proof-of-Stake: random selection based on current stake (coin holdings),
often using a "Follow-the-Satoshi" (FTS) algorithm
    - FTS is basically is just a hash function that takes some random input 
(e.g. previous block header, random string) and outputs an index. All tokens in
the system should be indexed, and the miner that currently holds the token with
the index output by FTS is elected the leader.
    - So Proof-of-Stake can also be thought of as **a weighted random coin-tossing
process**, but where having more stake in the system leads to a higher chance to win
- 2 major advantages:
    - Less computational power
    - Opportunity for faster transaction confirmation speed, higher transaction
throughput
- On the second point, Tx/s = (Block-size) / (Tx-size * Block-time)
    - PoS protocols usually have larger block size than the PoW-based Bitcoin
network (although I'm not clear on if this is an actual limitation of PoW in
Bitcoin or if it is simply an administrative decision) and shorter block time 
(e.g. the amount of time it takes to confirm a block, probably because there is
no expensive computation involved), which leads to much higher Tx/s
    - For reference: Bitcoin (with Block-size = 1MB, Tx-size=250bytes,
Block-time=600s) allows for roughly 7 Tx/s. Ouroboros, a formally defined PoS
protocol, is able to allow around 257 Tx/s.

### Stake Pools and Decentralization Analysis
This section is heavy on math/game theory and I don't completely understand it,
but the general takeaways can be summarized as follows:
- A PoS system can be modeled as a non-cooperative game, which models a game where
players have conflicting interests and act independently to mamximize their profit
- 2 desirable outcomes in a non-cooperative game:
    1. Dominant-strategy equilibrium: every player has a dominant strategy (e.g.
a strategy that outperforms every other possible option), equilibrium is reached
because every player will just choose the dominant strategy. However, dominant
strategies often do not exist. 
    2. Pure-strategy Nash equilibrium: every player cannot get a better payoff
by independently switching to other strategies. 
- The authors then model a PoS system as a game, and prove the following theorems
about the game:
    1. The game has at least one Nash equilibrium.
    2. The strategy s, where a player invests *all* of its budget, dominates
all strategies s', where a player invests *less* than its total budget.
    3. The game has a unique Nash equilibrium s-star and convergence to s-star
is guaranteed.
    4. Theorem 4: The Nash equilibrium of the game is Pareto-optimal (i.e. the
strategies which give a player the best payoff without decreasing the payoff
of other players).
- The authors then build an iterative algorithm that finds the Nash equilibrium
of the game, run a series of simulations using the algorithm, and arrive at the 
following good property about the game:
    - **While a pool's cost and fee are not controlled by the network providers,
the block reward and total network stake can be adjusted to maintain 
decentralization in the network**
    - Essentially, in an ideal PoS system where each players act rationally and
the Nash equilibrium is reached, it is possible to maintain decentralization in 
the system by adjusting system-level parameters: block reward and total network 
stake

### Outstanding Challenges for PoS Protocols
- Security issues: there are a number of security issues described that are 
open to further research
- Incentive compatibility: need to design incentive mechanisms where rewards for
good behavior outweigh the benefits of malicious behavior, not much research
has been done in this area, authors suggest a game-theoretical analysis of users'
rational behavior
- Protocol designs: authors want more rigorous analysis of the impact of each 
dimension of protocol design on important qualities like security, performance,
finality in order to inform better future designs
