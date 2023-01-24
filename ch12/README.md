- application logic usually involves multiple SQL statements to be executed together as a logical unit of work
- **tranasaction**: the mechanism used to group a set of statements together, such that either all or none of the statements succeed

# Multiuser Databases
- multiple users access a database
- what if data addition/modification/deletion occurs in such an environment?

## Locking
- to control simultaneous use of data resources
- when some portion of the database is locked, any other users wishing to modify(or read) that data must wait until the lock has been released
- most db servers use one of the two strategies:
    - a db writer request and receive from the server a *write lock* to modify data, and a *read lock* to query data
        - only one write lock is given out at a time for each table(or portion)
        - read can be done by multiple users, but those requests are blocked until the write lock is released
    - a db writer requests and receive from the server a *write lock*(the same as the above), but read doesn't need any lock
        - instead, the server ensures that a reader sees a consistent view of the data(even though other users may be making modifications),
        from the time her query begins until her query has finished(**versioning**)
    
- mysql uses both strategies depending on which *storage engine* you'll choose

## Lock Granularities
- how to lock? three different levels(or **granularities**)
    - table locks: keep multiple users from modifying data in the same table simultaneously
    - page locks: keep multiple users from modfying data on the same page
    - row locks: keep multiple users from modifying the same row in a table simultaneously

- mysql uses all three strategies depending on which *storage engine* you'll choose 

# What Is a Transaction?
- how can we guarantee that multiple users access the same data?
- concurrency issue: lock isn't enough - what if there is an error occurred? what if your page lock on a table times out?
- **transaction**: a device for grouping together multiple SQL statements such that either *all* or *none* of the statements succeed(**atomicity**)
    - `commit`: if everyting succeeds, we end the transaction
    - `rollback`: if sth unexpected happens, we instruct the server to undo all changes made since the transaction began

- regardless of whether a transaction was commmited or was rolled back, all resources acquired(e.g. locks) during the execution of the transaction are released,
  when the transaction completes

- if the server or program shuts down all of sudden before commit or rollback, the transaction also will be rolled back when the server comes back online
- if already a commit is issued but the server shuts down before the changes have been applied to permanent storage(the changed data is in memory but not
  flushed to disk), then the database server must reapply the changes from the transaction when it restarts(**durability**)

## Starting a Transaction
- a db server handles transaction creation in either of two ways:
    - no explicit transaction instruction
        - when the current transaction ends, the server automatcially begins a new transaction for your session
    - explicit transaction
        - individual SQL statements are automatically committed independently by default(**autocommit mode**)
        - to begin a transaction, you must first issue a command

- mysql takes the second approach: once you execute a transaction and its commit is made, then the change made will be permanent
    - you can choose to turn on/off the autocommit mode

## Ending a Transaction
- you must explictly end your transaction for your changes to become permanent: `COMMIT` command
    - instructs the server to mark the changes as permanent
    - release any resources(page or row locks) used during the transaction

- if you don't want the current change, simply `ROLLBACK`
    - likewise, it relesase any resources used by your session

- possible implict commit/rollback cases:
    - server shuts down -> rollback
    - issue `start transaction` command -> automatically commits the previous transaction
    - altering the table itself: cannot be rolled back, so it must ocurr outside a transaction -> automatically commits the previous transaction
    - deadlock detention -> roll back(the terminated transaction can be restarted later)    

## Transaction Savepoints
- maybe you don't want to undo all the changes made so far: make savepoints within the transaction and go back to those specific points
- you must name savepoints: otherwise they're all ignored

```sql
SAVEPOINT my_savepoint

ROLLBACK TO SAVEPOINT my_savepoint;
```
