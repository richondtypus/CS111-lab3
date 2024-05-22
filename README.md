# Hash Hash Hash
We implement a serial hash table implementation, along with two concurrent hash table implementations that use different locking strategies. The first version contains a single mutex lock whereas the second version contains a lock for each bucket.

## Building
To build run,
```shell
make
```

## Running
When we run the following program we yield the following results,
```shell
./hash-table-tester -t 8 -s 50000
Generation: 183,861 usec
Hash table base: 2,273,317 usec
  - 0 missing
Hash table v1: 2,967,302 usec
  - 0 missing
Hash table v2: 1,664,805 usec
  - 0 missing
```

## First Implementation
My initial implementation approach employs a single global lock for the entire hash table. This strategy ensures that only one thread can access the hash table at any given time, thereby preventing concurrent access issues like race conditions or deadlocks. While this method is simple to implement and comprehend, it can result in performance bottlenecks when multiple threads compete for the lock. Despite its potential inefficiency in highly concurrent settings, it effectively ensures thread safety for the hash table.

### Performance
When we run the following program we yield the following results,
```shell
./hash-table-tester -t 8 -s 50000
Hash table base: 2,273,317 usec
  - 0 missing
Hash table v1: 2,967,302 usec
  - 0 missing
```
Clearly version 1 is a little slower/faster than the base version. As the number of threads increases, the numbe of threads competing for the same lock also increases. This drastically inhibits performance as we now have more threads waiting for the same lock. For adding one lock however, this solution is correct because even though it is inefficient with multiple threads, it ensures that only one thread at once can update the hash-table.

## Second Implementation
In the `hash_table_v2_add_entry` function, I added a lock to each bucket. The idea is to allow multiple threads to work concurrently better. By only locking one bucket at a time, the other threads do not have to wait for a thread already holding the lock to give it up. Instead they can move to a different bucket and update that concurrently.

### Performance
When we run the following program we yield the following results,
```shell
./hash-table-tester -t 8 -s 50000
Generation: 183,861 usec
Hash table base: 2,273,317 usec
  - 0 missing
Hash table v2: 1,664,805 usec
  - 0 missing
```
We can clearly see a significant performance improvement with the second version of the hash table. However, now we see that increasing the number of threads improves performance. This is because every thread does not have to wait for the same lock. Concurrency is much better since every lock can just access a bucket with an open lock. This implementation is correct too because we are ensuring that each thread can only access an unlocked bucket and lock and update it. Each bucket will only get updated once, and thus we can ensure that it is updated correctly. 

## Cleaning up
To clean up run,
```shell
make clean
```
