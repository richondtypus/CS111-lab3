# Hash Hash Hash
We implement a serial hash table implementation, along with two concurrent hash table implementations that use different locking strategies. The first version contains a single mutex lock whereas the second version contains a lock for every entry in the hash table.

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
In the `hash_table_v1_add_entry` function, I added TODO

### Performance
```shell
TODO how to run and results
```
Version 1 is a little slower/faster than the base version. As TODO

## Second Implementation
In the `hash_table_v2_add_entry` function, I TODO

### Performance
```shell
TODO how to run and results
```

TODO more results, speedup measurement, and analysis on v2

## Cleaning up
To clean up run,
```shell
make clean
```
