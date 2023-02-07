# MongoDB

## Paper
- mongoDB@SIGMOD2019 

## Satisfied Model
- strong causal convergence (SCCv) **?**

## System Model
- shared clusters
  - each shard represents a horizontally partitioned set of data (non-overlapping *key* domains)
  - each shard has a primary and one or more secondary nodes
     - primary node: receive and compute all writes  
     - secondary node: replica all writes from the primary node in the same shard. 
- routers: route client commands, not hold data     
- clients: application processes
  
## Implementation

### Logical Clock

- clustertime
  - content
      - hybrid logic clock: <*time, counter*>
          - *time*: physical time
          - *counter*: deal with the case where writes have the same *time* part
  - ticks for a new write (only on primary node) <*t,c*>     
      - computes the current physical time (*lc*)    
      - compare the received clustertime (*rc*) with the local one (*lc*)   
          - *rc > lc*          
              -  set *t* as *rc.time*; 
              -  set the *c* as *rc.counter +1*      
          - *rc <= lc*       
              -  set *t* as *lc*             
              -  set *c* as 0   
  - distribution 
      - primary node receives a write *w1* from client *c1*         
          - advances its clustertime, if the received one is greater          
          - ticks a new clustertime *t1*          
          - adds the *w1* with *t1* to the *oplog*
          - returns *w1* with *t1* to the client *c1*
      - client receives the ack of *w1* from the primary node 
          - advances the local clustertime to *t1*  
  
### Causal Dependency 

- shard receives a read request (including the eaxct written time *wt* of *key*) from a client 
  - shard has the *key* 
    - primary node: returns the value  
    - secondary node: checks its *oplog*       
      - has the data with *wt*: returns results    
      - has no data with *wt*: waits for the replication from the primary node; returns until it is replicated
  - shard contains no such *key*
      - The choice of shard key cannot be changed after sharding. A sharded collection can have only one shard key. See Shard Key Specification ([https://docs.mongodb.com/v3.6/sharding/#shard-keys](https://docs.mongodb.com/v3.6/sharding/#shard-keys "shared keys"))   
      - **?** router should route the request to the shard which has the *key*, this case should not happen

# SwiftCloud

## Paper

- [https://pdfs.semanticscholar.org/9f0c/7b6796eff5dd3bd909788ad645039a765091.pdf](https://pdfs.semanticscholar.org/9f0c/7b6796eff5dd3bd909788ad645039a765091.pdf "Dependable eventual consistency with replicated data types")

## Satisfied Model

- Weak Causal Convergence (WCCv)
  - definition 6.1 

## System Model

- data centers (DC)  
  - **full** replica of the database
  - communicate in a peer-to-peer way
  - a DC may fail and recover with its persistent memory intact  
- clients
  - communicate only via DCs
      - normally, a client connects to a single DC
      - in case of failure or roaming, a client connects to zero or more DCs
  - **partial** replica of the database
      - containing only the small subset of the database of current interest to the local app

## Implementation

### Timestamp and Version Vector

- timestamp
  - each timestamp is a pair *(i,k)* 
      - *i*: the node who assigned it
      - *k*: sequence number
  - each update has a set of timestamps
      - *Tc*: unique client-assigned timestamp
      - *Tdc*: a set of zero or more DC-assigned timestamps
          - no DC timestamp before being delivered to a DC, and one thereafter
          - more than one in case of delivery to multiple DCs (on failover)
- vesion vector
  - a partial map from node id to integer, e.g. [DC1->1, DC2->2]
  - at most one client entry, and multiple DC entries
  - uses version decoding function to drive updates in a certain  version
  
### Protocol

#### Initially

- data centers
  - has a state *Udc*, which is an empty log of updates 
  - has a zeroed version vector *Vdc*
- clients
  - has a projection *Vdc|o* of *Vdc* (based on its interest operations *o*) 
  - a copy of vector *Vdc*
  - a commit log of its own updates *Uc*

#### Client-Side Execution

- an internal update
  - assigns it client timestamp *(c,k)*
  - assigns it a dependency vector *VVdeps*
  - append it to the commit log of local updates *Uc*
  - triggers transfer process

#### Transfer from Client to Data Center (background)

- client side (*u*’s internal dependencies has recently been assigned a DC timestamp)
  - merges this timestamp into the dependency vector *u.VVdeps*
  - sends a copy of *u* to its current DC and wait for acknowledgement
      - receives an acknowledgement from the DC
          - records the DC timestamp(s) in the original update record *u.Tdc*
      - timeout: the DC is suspected unavailable
          - the client connects to another DC (failover) and repeats the protocol
      - DC reports a missing internal dependency
          - marks as unacknowledged all internal updates starting from the oldest missing dependency, and restarts the transfer protocol from that point
      - DC reports a missing external dependency
          - the client tries another DC

- DC side
  - verifies if *u*'s dependencies are satisfied
      - if fails, reports an error to the client (missing internal/external updates)
      - else, continues
  - deals with the update
      - received this update for the first time
          - assigns it a DC timestamp *{(DC,vvdc(dc)+1)}*
          - stores it in its durable state *Udc*
          - incorporates its timestamp to *vvdc*
          - triggers geo-replication
      - received this update previously
          - looks up its previously-assigned DC timestamps
  - DC acknowledges the transfer to the client with the DC timestamp(s)
   

#### Geo-replication from DC to DC

- DC which reveices a fresh update 
  - broadcasts it to all other DCs
  - stores it in replication log until every DC receives it
- DC reveices a broadcast update
  - if the dependencies of u are not met, buffers it until they are
  - incorporate u into durable state *Udc*
  - incorporate its timestamp(s) into the DC version vector *VVdc*
  - triggers notification

##### Notification from DC to Clients

- DC sends an acknowledgement to client
  - includes the new version *VVdc*
  - includes a log of all the nterest updates between the client's previous base version and new version *VVdc*
- client receives the acknowledgement  
  - applies the newly-received updates to its local state 
  - updates the base version to *VVdc*