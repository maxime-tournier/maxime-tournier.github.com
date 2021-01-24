---
title: Concurrent ML
categories: [prog]
---

- optimization: split *optimistic* and *pessimistic* phases to skip
  waiting
  - optimistic
    - non-blockingly check queue for waiting items
	- complete transaction if found
  - otherwise, pessimistic: 
     - suspend execution until someone else completes the transaction,
       which will resume us
     - publish ourselves as waiting in queue
       - (could be done in reverse order with proper locking)
- but this introduces a possible deadlock if not careful:
  - nobody is waiting in queue during the optimisic phase on both
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
  - the other side may attempt to complete our transaction during its own
    re-check (C2)
  
so we need some way of publishing that we're rechecking
(symmetrically, detect that the other side is rechecking), and act
accordingly:

- the other side will not attempt to optimistically complete a
  transaction currently being rechecked, so C1 cannot happen
- the other side will not consider a transaction currently being
  rechecked during its own recheck phase, so C2 cannot
  happen. instead, we'll wait (spin) until recheck is over.


to this end we introduce the following states:

- `W`: waiting (initial state, when published)
- `C`: claimed (when re-checking)
- `S`: synched (when complete)

this side will be `A`, the other side will be `B`. after publishing,
the following transitions are possible on side `A`:

- `W -> S` (`B` completed during its optimistic phase)
- `W -> C` (`A` claimed)
- `C -> W` (`A` rolled back)
- `C -> S` (`A` completed during recheck, after claiming)


## recheck
  
- if empty queue: wait
- else try to claim our side `A: W -> C`
     - otherwise `A: S`: optimistically completed by other side
	    - unpublish our side
		- complete transaction
	 - otherwise `A: C`: impossible
     - success: try to `B: W -> S` the other side (queue head)
	   - success: 
		  - complete our side `A: C -> S`
		  - unpublish our side
		  - resume other side
		  - complete transaction
	   - otherwise `B: S`: somebody else completed the other side
	 	 - rollback `B: C -> W`
		 - retry
	   - otherwise `B: C`: somebody else is already claiming the other side
		 - let them handle it
		 - retry


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
