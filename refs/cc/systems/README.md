# systems

## Papers

### MongoDB
See [mongodb-causal-consistency](https://github.com/hengxin/mongodb/blob/master/mongodb-docs/mongodb-causal-consistency.md)

### NSDI
- [NSDI2017 (Wyatt Lloyd): I Can't Believe It's Not Causal! Scalable Causal Consistency with No Slowdown Cascades]()
- [NSDI2013 (Wyatt Lloyd): Stronger Semantics for Low-Latency Geo-Replicated Storage]()
> `Eiger`. The primary contributions of this work are enabling scalable causal consistency 
for the complex column-family data model, as well as novel, 
non-blocking algorithms for both read-only and write-only transactions.

### SOSP
- [SOSP2011 (Wyatt Lloyd): Do Not Settle for Eventual Scalable Causal Consistency for Wide-Area Storage with COPS]()
> `COPS`. In this paper, we identify and define a consistency model: 
causal consistency with convergent conflict handling, or ***causal+***;
> We present the design and implementation of *COPS*,
a key-value store that delivers this consistency model across the wide-area.

### EuroSys
- [EuroSys2017 (Manuel Bravo): Saturn A Distributed Metadata Service for Causal Consistency]()

### TOCS
- [TOCS2011: Depot: Cloud Storage with Minimal Trust]()
  > FJC: Fork-Join-Causal Consistency

### ICDCS
- [ICDCS2016 (Manuel Bravo): Cure Strong Semantics Meets High Availability and Low Latency]()

### ATC
- [ATC 2017 (Manuel Bravo): Unobtrusive Deferred Update Stabilization for Efficient Geo-Replication]()

## Researchers
- [Wyatt Lloyd](https://www.cs.princeton.edu/~wlloyd/)
- [Manuel Bravo @ IMDEA Software Institute](https://sites.google.com/view/manuelbravo)

## Source Code

## Articles
- [Causal Consistency (Jan Lindstrom; 2015-02-22)](https://mariadb.org/causal-consistency/)
> COPS, Eiger, Bolt-on Causal Consistency
> Massively Multi-player Online Role-Playing Games (MMORPG)
  - [(2013) Consistency Models for Cloud-based Online Games: the Storage System's Perspective](https://pdfs.semanticscholar.org/e371/b40896e63bef3592e971773055c304c6011a.pdf?_ga=2.116073605.2083703173.1580642097-1344665591.1580642097)
> Facebook
