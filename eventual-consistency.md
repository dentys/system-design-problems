Eventual Consistency

**What is data consistency**

- General integrity of the data, its valid state according to existing constraints and rules (Consistency as “C” in “ACID” transaction properties)
- Synced state of all nodes in a distributed system

Maintaining consistency is a particular challenge for **distributed systems** due to the additional network layer which imposes potential high latencies and connection failures, especially when it comes to global-scale networks.

Types of consistency in distributed systems:
Strong consistency: guarantees that every read operation returns the latest write operation’s result, regardless of the node on which the read operation is executed. Users can always read the most recent write, and there are no stale or conflicting values
Eventual consistency: allows temporary inconsistent state of the system, implying that it will achieve consistency over time. Users may sometimes receive stale data on their requests.

**CAP theorem**

The theorem says a distributed system can guarantee only two of the following three features at the same time:

**C for Consistency** — Every read receives the most recent write or an error.

**A for Availability** — Every request receives a response, without guarantee that it contains the most recent version of the information.

**P for Partition Tolerance** — The system continues to operate despite arbitrary partitioning due to network failures.

The network is never 100% reliable, so **partition tolerance is a must** - therefore the choice is between CP and AP.

In terms of CAP theorem, strong consistency is represented as CP option, and eventual consistency is AP.


|                                      | Strong consistency                | Eventual consistency                   |
|--------------------------------------|-----------------------------------|----------------------------------------|
| Data integrity                       | Reads always return latest values | Reads may return stale values          |
| Propagation of updates               | Synchronous                       | Asynchronous                           |
| Latency                              | Higher                            | Lower                                  |
| Availability (on network partitions) | Unavailable                       | Available                              |    
| Use cases                            | Financial services, ERP           | Social media platforms, Search engines |


**Ways to ensure strong consistency in distributed systems:**

**Distributed Locking**
A technique used in distributed systems to prevent different processes from accessing or changing shared data simultaneously.
A typical distributed locking system consists of multiple nodes and a lock manager. The lock manager maintains the status of locks and handles lock and release requests from nodes. Nodes communicate with the lock manager to gain access to shared resources.

**Two-Phase Commit (2PC)**
This protocol ensures atomicity in distributed transactions. It involves a coordinator who communicates with all nodes participating in a transaction. In the first phase, nodes agree or disagree to commit, and in the second phase, the actual commit or rollback occurs based on the first-phase responses.

**Paxos Protocol**
Paxos is a consensus algorithm used to achieve agreement among a network of distributed nodes. It ensures that a group of nodes reaches consensus on a single value, even if some nodes may fail or experience delays.

**Quorum-based Systems**
In systems using quorums, a certain number of nodes must agree on an operation for it to be considered successful. This approach allows for flexibility in terms of availability and partition tolerance while still providing a level of consistency.

**Raft Consensus Algorithm**
Similar to Paxos, Raft is another consensus algorithm that ensures the consistency of distributed systems. It simplifies the understanding and implementation of distributed consensus compared to Paxos.

**Microservice patterns that employ eventual consistency**

**Event Sourcing / CQRS**

Event Sourcing ensures that all changes to application state are stored as a sequence of events. Not just can we query these events, we can also use the event log to reconstruct past states, and as a foundation to automatically adjust the state to cope with retroactive changes.
CQRS-based systems use separate read and write data models, each tailored to relevant tasks and often located in physically separate stores. When used with the Event Sourcing pattern, the store of events is the write model, and is the official source of information.

Applying write model data to read model oftentimes happens asynchronously, which implies eventual consistency in the system.

**SAGA**

A Saga is a sequence of local transactions. Each local transaction updates the local database using ACID transactionі and publishes an event to trigger the next local transaction in the Saga. If a local transaction fails, then the Saga executes a series of compensating transactions that undo the changes, which were completed by the preceding local transactions

