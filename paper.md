A Zero-Trust Peer-to-Peer Distributed Computing Protocol
========================================================

Authors: Josh Orndorff, Mitchell Pavlak, Michael Sun, Andrew Beckmen 

Organization: The Pingry School

Special Thanks: Colleen Kirkhart, Ellen Li, (TODO others ?)


Abstract
--------
We present a network protocol in which peers can collaborate to perform calculations for one another in a decentralized way. Peers need not trust one another, nor any central authority to ensure that the results they receive are correct. Peers identify one another by unique public keys, and records of work performed are kept in a time-stamped block chain. The block chain allows the contributions and uses (TODO vocab) of each node to be logged to ensure equitable work distribution. This work can be thought of equivalently as a resource sharing system akin to bittorrent where nodes share clock cycles rather than data, or a digital cryptocurrency akin to bitcoin where the currency is the computation power itself.


Introduction
------------
(TODO write this. Probably worth referencing the following idea fragments)
* P2P is popular like in bittorrent and bitcoin
* Parallel computing is awesome, and useful for many applications - the success of map reduce proves it. But only big organizations can afford dedicated grids; this empowers the little guy.
*(TODO other ideas)

This paper discusses such a protocol at a conceptual level. Full technical details of our implementation along with a proof-of-concept client are available and discussed briefly at the end of the paper.


Proposing, Accepting, and Completing Jobs
-----------------------------------------
(TODO write this. The section should basically outline the following, all of which will need to be expanded)
* The master conceives of a problem and writes a parallel algorithm to solve it. Libraries like Map Reduce could be quite helpful here and that should be mentioned
* The master will decide how to confirm correct results according to the earlier section about trust and reliability)
* The master will broadcast request(s) (TODO vocab) for peers in the network to accept
* The workers will complete their tasks, but not return the result until the master has signed a completion.
* The master will sign the completion only after the workers have proved that they have completed the work correctly.
* The previous two points may seem like a standoff, but it isn't. This is where ZKP comes in. After the master is convinced the work was done correctly, both parties will sign the completion and broadcast it to be hashed into the blockchain.
* After the completion clears the block chain the worker will return the results to the master.


Transaction History
-------------------
### Timestamp Server
A problem that can arise in distributed networks is the issue of utilizing resources multiple times by broadcasting messages simultaneously. As described in the Bitcoin protocol, a solution to this problem is to employ a timestamp server that assigns a timestamp to each block within the blockchain. The inclusion of a timestamp verifies that data existed at a certain point, and each timestamp references the previous timestamp in its hash. This ensures the linear progression of the block chain by reinforcing prior timestamps.   

### Types of Transactions
(TODO write this out)
* Propose
* Accept
* Complete -- Maybe the worker can just directly inform the master upon work completion? Does it need to be publicly timestamped?
* Verify -- Signed by the master confirming that the work has been done and updating the reputations of both master and worker.
* Abort -- Can be sent by a worker when they no longer wish to perform the work or by the master if the worker is unresponsive for a certain pre-arranged amount of time (eg. worker experience a power failure)
* Report -- For reporting cheaters

### Incentive
One weakness of other systems that use a hash-based proof of work for time-stamping (eg bitcoin) is that they spend a huge amount of computation power on performing hashes which corresponds to spending real-world resources like energy. Wasting computation power is never ideal, especially in a network designed specifically to equitably share that resource. A property of this network (TODO For real, we need a name!) is that the network will self-regulate a worker's incentive.

In general a worker can gain reputation by either, solving real problems for a master, or hashing blocks into the block chain, and a history of their choices will be maintained in their reputation score. When choosing workers to complete their jobs, a master will be able to screen potential workers based on their reputation score. So in the event that no one is hashing transactions into the blockchain, masters may reward nodes who have worked on the block chain recently by giving them more work, and refrain from giving work to nodes who have not pulled their share of the weight: thus incentivizing more hashing into the blockchain. On the other hand, if too many peers have been competing to hash blocks into the block chain, masters can reward nodes who have done real work with more jobs in the future. Masters are incentivized to do this, because inefficiencies in the spread of computational power over the different jobs ultimately hurts the master. 

(TODO CURRENCY? Allowing different payments based on reputation scores/other metrics —> one example is instead of not accepting the work of nodes that don’t pull their weight in one of the above, masters could just charge them a surcharge?)


Trust and Reliability
---------------------
Workers may be motivated to "cheat" by claiming to have completed calculations and returning bogus results in order to boost their reputation. Here are some ways to prevent cheating.

### Zero Knowledge Proof
(TODO write this)

### Redundant Execution
Executing a single operation across one or more nodes may be an adequate way to ensure that a node does not have the ability to claim false results. Since an untrusted node may be assigned a large number of operations to complete per program, if a certain percentage is repeated and confirmed to be correct by a reliable node, it becomes unlikely that node cheated on a percentage of tasks that would allow them to gain any real benefit in processing power, all of which happened to be those that were not double-check. This is because if a single operation is found to be false on a single program, all operations signed with that signature could be invalidate, making the risk-reward lean towards honestly completing the assigned work. While redundant execution does require more work, it allows a master node to be relatively certain of their results. Ultimately, the amount of redundancy in each program operation will be up to the master node to decide when making their program request (TODO vocab).



Reputation System
-----------------
A key component of the network is that peers do not trust one another. The network’s reputation system is a decentralized metric that allows a node to gauge the trustworthiness of a peer. Transactions are signed using a public-private key pair, and for each unique public-private key, the network stores: 
(TODO review introduction)

For each node, which will be uniquely identified by a public-private key pair, the reputation system keeps track of the following.
* Operations performed as a worker
* Operations received (TODO vocab) as a master
* Times reported as a cheater
* Number of cheaters reported
* Amount of work performed to support the network (eg. hashing transactions into the block chain.)
* (TODO Currency System?)
* (TODO benefits of a currency such as self balancing the blockchain vs problems, and providing an incentive to be honest)


Security and Privacy
--------------------
As with all situations where data are communicated to untrusted parties, data security is paramount.

### Workers' Concerns
While some suspect that the major threat is for workers who may in principal be tricked into executing malicious code, these concerns are quickly dismissed. A worker will be performing operations on data supplied by a master and returning results to the master. Under no circumstances should a worker combine its own data with that of the master's problem, and thus network calculations (TODO need better vocab here) should be made in an isolated environment. In enterprise settings, this may mean running on dedicated hardware with no private data accessible to the machine at all. In a personal setting it likely means running in a sandbox. Further, nothing precludes a worker from comparing jobs to a database on known malicious signatures (ie "antivirus") of its choosing.

### Masters' Concerns
The real data privacy concern is on the part of the master who, in general, will need to provide data to the workers who may try to steal or misuse it. These concerns are more complex and may be partially addressed by some of the following techniques. But ultimately it is the duty of the master to ensure cryptographic security of its own data before giving it out to untrusted peers.

#### Homomorphic encryption
https://en.wikipedia.org/wiki/Homomorphic_encryption
(TODO fill in details here)

#### Data scattering
This basically means providing such little data to any individual peer that the data is of no use
(TODO fill in details here)

#### Other methods?
(TODO are there any?)


Proof of Concept
----------------
We have implemented a platform independent proof-of-concept client in Python to demonstrate the feasibility of such a network. The complete code is available at (TODO put link here). This code works well, but has not been analyzed by a security expert and is not recommended for production use.

Discuss accomplishments our code achieves, assumptions it makes, and weaknesses it has.

Technical details of our implementation are available at (TODO put link here. Could be the same link as earlier in this section, but let's decide after we get it working.)


Conclusion
----------
(TODO Is this necessary? If so, write it)


References
----------
1. List
2. them
3. here
