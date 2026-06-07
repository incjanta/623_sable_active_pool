# Trust-model notes

## ActivePool and core custody routing

`ActivePool.sendBNB()` is callable only by `BorrowerOperations`, `TroveManager`, or `StabilityPool`, and its `receive()` path only accepts BNB from `BorrowerOperations` or `DefaultPool`. On the live deployment, the pool wiring points to the expected Sable core addresses and the contract is not a proxy.

## Governance and ownership boundary

The core pool contracts queried during this run report `owner = address(0)`, which removes straightforward owner-only rug paths from the live deployment. `SystemState` still contains timelock-governed configuration powers, but those remain inside the trusted governance boundary for this workflow.

## Oracle notes

`PriceFeed.fetchPrice()` is restricted to the core contracts rather than being directly public. I did not confirm a public oracle-manipulation path that lets an untrusted actor drain `ActivePool` or `StabilityPool` value on this deployment.
