---
title: Concurrent ML
categories: [prog]
---

## states

- `W`: waiting
- `C`: claimed
- `S`: synched

## transitions

- `W->S`
- `W->C`
- `C->W`
- `C->S`

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
    - success: our state may no longer change until we do
      - walk the queue until `cas(W->S)`
		- success: 
		  - pop
		  - complete tx
		  - unpublish ourselves/mark as `S`
		- otherwise:
		  - empty queue: suspend
	- otherwise:
	   - `S`: someone else completed us, hence popped us
	     - complete tx by returning
	   - `C`: someone else is claiming this tx, spin



## invariants
  - we only ever pop `S` states
