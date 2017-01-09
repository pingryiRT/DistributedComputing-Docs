A Zero-Trust Peer-to-Peer Distributed Computing Protocol
========================================================

Authors: Josh Orndorff, Mitchell Pavlak, Michael Sun
Organization: The Pingry School
Special Thanks: Colleen Kirkhart, Ellen Li, (TODO others ?)


Abstract
--------
We present a network protocol in which peers can collaborate to perform calculations for one another in a decentralized way. Peers need not trust one another, nor any central authority to ensure that the results they receive are correct. Peers identify one another by unique public keys, and records of work performed are kept in a time-stamped block chain. The block chain allows the contributions and uses (TODO vocab) of each node to be logged to ensure equitable work distribution. This work can be thought of equivalently as a resource sharing system akin to bittorrent where nodes share clock cycles rather than data, or a digital cryptocurrency akin to bitcoin where the currency is the computation power itself.


Introduction
------------
(TODO write this. Probably worth referencing the following idea fragments)
* P2P is popular like in bittorrent and bitcoin
* Parallel computing is awesome, and useful for many applicaitons - the success of map reduce proves it. But only big organizations can afford dedicated grids; this empowers the little guy.
*(TODO other ideas)

This paper discusses such a protocol at a conceptual level. Full technical details of our implementaion along with a proof-of-concept client are available and discussed briefly at the end of the paper.


Transaction History
-------------------
(TODO talk about the blockchain-like system that we will use)


Trust and Reliability
---------------------
Workers may be motivated to "cheat" by claiming to have completed calculations and returning bogus results in order to boost their reputation. Here are some ways to prevent cheating.

### Zero Knowledge Proof
(TODO write this)

### Redundant Execution
(TODO write this)


Completing a Job
----------------
(TODO write this. The section should basically outline the following, all of which will need to be expanded)
* The master conceives of a problem and writes a parallel algorithm to solve it. Libraries like Map Reduce could be quite helpful here and that should be mentioned
* The master will decide how to confirm correct results according to the earlier section about trust and reliability)
* The master will broadcast request(s) (TODO vocab) for peers in the network to accept
* The workers will complete their tasks, but not return the result until the master has signed a completion.
* The master will sign the completion only after the workers have proved that they have completed the work correctly.
* The previous two points may seem like a standoff, but it isn't. This is where ZKP comes in. After the master is convinced the work was done correctly, both parties will sign the completion and broadcast it to be hashed into the blockchain.
* After the completion clears the block chain the worker will return the results to the master.


Reputation System
-----------------
(TODO write a sentence or two intro to this section)

For each node, which will be uniquely identified by a public-private key pair, the reputation system keeps track of the follwoing.
* Operations performed as a worker
* Operations received (TODO vocab) as a master
* Times reported as a cheater
* Number of cheaters reported
* Amount of work performed to support the network (eg. hashing transactions into the block chain.)
* (TODO what else?)


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
We have implemented a platform independant proof-of-concept client in Python to demonstrate the feasibility of such a network. The complete code is available at (TODO put link here). This code works well, but has not been analyzed by a security expert and is not recommended for production use.

Discuss accomplishments our code acheives, assumptions it makes, and weaknesses it has.

Technical details of our implementation are available at (TODO put link here. Could be the same link as earlier in this section, but let's decide after we get it working.)


Conclusion
----------
(TODO Is this necessary? If so, write it)


References
----------
1. List
2. them
3. here
