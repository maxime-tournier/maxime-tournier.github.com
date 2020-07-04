---
title: Concurrent ML
categories: [prog]
---

- optimization: split *optimistic* and *pessimistic* phases to skip
  waiting
  - optimistic: non-blockingly check queue for waiting items, complete
    transaction if found
  - otherwise, pessimistic: 
     - suspend execution until someone else completes the transaction,
       which will resume us
     - publish ourselves as waiting in queue
       - (could be done in reverse order with proper locking)
- but this introduces a possible deadlock if not careful:
  - nobody is waiting in the queue during the optimisic phase on both
    sides, so both sides suspend hence nobody will resume the other (oh noes!)
- so we need to recheck *after* publishing ourselves as waiting,
  *before* actually suspending
  - if waiting items found:
	 - unpublish ourselves
     - complete transaction
  - else: suspend
- but now re-checking is concurrent with other side's optimistic phase
  and re-checking. that is, while we re-check:
  - the other side may optimistically complete our transaction (C1)
  - the other side may consider our transaction during its own (C2)
    re-check. we're both published as waiting and rechecking.
  
so we need some way of detecting what's going on, and act accordingly.
to this end we introduce the following states:

- `W`: waiting (initial state, when published)
- `C`: claimed (when re-checking)
- `S`: synched (when complete)

together with the following possible transitions:

- `W -> S`
- `W -> C`
- `C -> W`
- `C -> S`

so now, after publishing ourselves as waiting in the queue, we must
mark our transaction as claimed as we enter re-checking, but *only if
we were still waiting*. so we use *compare-and-swap* `cas(W -> C)` and
proceed with rechecking only on success. this will fail only when:

- `S` the other side has optimistically completed our transaction (C1)
- `C` the other side has marked us as claimed (C2)




## 
- D1:
  - first try to claim this side `W -> C`
     - otherwise `S`: optimistically completed by other side
	 - otherwise `C`: impossible since nobody ever claims the other side
  - then try to `W -> S` on the other side
     - success: 
	     - complete our side `C -> S`
		 - unpublish our side
		 - resume other side
		 - complete transaction
	 - otherwise `S`: somebody else completed the other side
	   - rollback `C -> W`
	   - retry
	 - otherwise `C`: somebody else is already claiming the other side
	   - let them handle it
	   - retry
  
deadlock:
  - both sides get claimed
  - retrying

- D2:
  - first mark the other side as claimed `W -> C`
  - then claim our side


## optimistic phase

- walk the queue until `cas(W->S)`
  - success: 
    - pop
	- complete tx
	

## pessimistic phase

- publish ourselves as `W` (our state may change soon after)
- must now recheck queue for waiting states (otherwise potential deadlock)
  - claim our tx `W -> C` (otherwise someone else could complete us
    while we complete someone else)
    - success: our state may no longer change until we decide it to
	  - if empty queue: suspend
	  - else `cas(W->S)` on queue head
		- success: 
		  - pop
		  - complete tx
		  - unpublish ourselves/mark as `S`
		- otherwise:
		  - rollback `C->W`
		  - retry
		  
	- otherwise:
	   - `S`: someone else completed us, hence popped us
	     - complete tx by returning
	   - `C`: someone else is claiming this tx, spin



## invariants
  - we only ever pop `S` states
