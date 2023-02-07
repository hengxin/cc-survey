# clocks

## Papers

### Overview Papers
- [IEEE Data Engineering Bulletin 2015 (Manuel Bravo): On the use of Clocks to Enforce Consistency in the Cloud]()
> The purpose of this paper is to revisit these concepts (***different kinds of clocks: scalar/vector/matrix logical/physical clocks***) in the concrete setting of developing distributed data store systems for the cloud.

### In Chronological Order:
- [CACM1978: Time, Clocks, and the Ordering of Events in a Distributed System]()
> Lamport Clock (scalar logical clock)
    - [Lamport timestamps @ wiki](https://en.wikipedia.org/wiki/Lamport_timestamps)
    - [Lamport's Logical Clocks @ Blog](https://mwhittaker.github.io/blog/lamports_logical_clocks/)

- [Fidge: Timestamps in Message-Passing Systems That Preserve the Partial Ordering]()
- [1989: Virtual Time and Global States of Distributed Systems]()
- [Fidge@IEEE Computer1991: Logical  Time  in  Distributed  Computing  Systems]()
- [Mattern@JPDC1993: Efficient Algorithms for Distributed Snapshots and Global Virtual Time Approximation]()
> Vector Clock (vector logical clock)

- [arXiv2010: Dotted Version Vectors: Logical Clocks for Optimistic Replication]()
> In this paper, first we review, and expose problems with current approaches to causality tracking in optimistic replication: these either lose information about causality or do not scale, as they require replicas to maintain information that grows linearly with the number of clients or updates. Then, we propose a novel solution that fully captures causality while being very concise in that it maintains information that grows linearly only with the number of servers that register updates for a given data element, bounded by the degree of replication.

- [OPODIS2014: Logical Physical Clocks](https://cse.buffalo.edu/~demirbas/publications/hlc.pdf)

> ... we propose a hybrid logical clock, HLC, that combines the best of logical clocks and physical clocks. HLC captures the causality relationship like logical clocks, and enables easy identification of consistent snapshots in distributed systems.

