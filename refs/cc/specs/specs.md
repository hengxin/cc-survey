# specs

## supporting data types
- shared register
  
  + CM@DC95, OCC@PODC15

- arbitrary data types
  
  + others

## relative strength  
("=" represents equivalent)

- WCC@PPoPP16 = CC@VCC17
 
- CC@PPoPP16 = CM@VCC17

- CM@DC95 = CC@PPoPP16 (condition: distinct written values)
 
- WCCv@PPoPP16 = CCv@VCC17 = CC@POEC15

- COCV@UEC13 = Causal+@SOSP11 = **WCCv@(vis,ar_l,ar_g) framework**

- RTC@TR11 is the strongest achievable causality based consistency in high-available, convergent systems

## variants

### causal+ consistency
- [SOSP11-Do not Settle for Eventual Scalable Causal Consistency for Wide-Area Storage with COPS](https://github.com/hengxin/causal-consistency/blob/master/models/SOSP11-Causal%2B-Do%20not%20Settle%20for%20Eventual%20Scalable%20Causal%20Consistency%20for%20Wide-Area%20Storage%20with%20COPS%20.pdf)

- key-value store
  * put(key,val)
  * get(key)  
- causal order
  * put-to order 
- convergent conflict handling
  * handler thread
      + *convergent conflict handling function*: operates on a set of conflicting operations on a key to eventually produce one (possibly new) final value for that key.
      + add new generated causal orders between the following two elements: 1. original *puts* or intermediate values; 2. the final value
  * client thread
      + conflict-free *put* set 
- informal definition
  * for each **client** thread *t* of an execution, there is a serialization of **all local operations** on *t* and all conflict-free ***puts***, that respects the causal order.
