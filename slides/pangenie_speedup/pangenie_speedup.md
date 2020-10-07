# Count and acces count speeder

----

## Kmer counting

kmer: a sub sequence of length k

Sequence: ACCATAGATTAG

5-mer:
- ACCAT
- CCATA
- CATAG
- …

----

## Cache Coherence trouble

Imagine we have a associative function convert kmer in index 
We can use this index to count occurence of kmer.

If this hash function convert ACCAT -> 2 and CCATA -> 3200302
A cache is generate

---

# Count solution

----

# Jellyfish

Use a multithread hash table to count kmer.

Pro:
- it's already in place
- easy to use

Cons:
- large memory usage
- cache miss

----

# Minimal Perfect Hash

For a set of fixed kmer, build a minimal perfect hash and use it to count kmer

Pro:
- small memory usage 9.3 bytes per kmer (in theory)

Cons:
- computation time
- cache miss


----


# Quotient Filter

A hash table with some false postive rate

Pro:
- low memory usage
- fastes ones

Cons:
- false positive rate
- some strange error need to be fix
- cache miss

----

# Brisk

A cache coherent associative structure.

Pro:
- low memory usage 2 bytes per kmer (in theory)
- cache coherent

Cons:
- not yet avaible

---

# Experiment

I have three different step:
- indexing, kmer we want count
- count, indexed kmer of a dataset
- dump, read a sequence file and dump the count of each kmer seens

Test dataset E. coli, 50x illumina, 4,687,524 uniq kmer


| | Time | Memory | per kmer |
|:-|-:|-:|-:|
Jellyfish count | 1m | 0.22Gb | 47 bytes | 


----

## Index

| | Time | 
|:-|-:|
| MQF | 12s |
| MPHF | 2s |
| Brisk | $\emptyset$ |

| | Memory | per kmer | 
|:-|-:|-:|
| MQF | 102 Mb | 22 bytes |
| MPHF | 87 Mb | 18 bytes |
| Brisk | $\emptyset$ | $\emptyset$ |

----

## Count

| | Time | 
|:-|-:|
| MQF | 6 s |
| MPHF | 27s |
| Brisk | $\emptyset$ |

| | Memory | per kmer | 
|:-|-:|-:|
| MQF | 102 Mb | 22 bytes |
| MPHF | 48 Mb | 10 bytes |
| Brisk | $\emptyset$ | $\emptyset$ |

----

## Dump

| | Time | 
|:-|-:|
| MQF | 6s |
| MPHF | 5s |
| Brisk | $\emptyset$ |

| | Memory | per kmer | 
|:-|-:|-:|
| MQF | 112 Mb | 24 bytes |
| MPHF | 57 Mb | 12 bytes |
| Brisk | $\emptyset$ | $\emptyset$ |


----

## Index human genome

Human dataset from Jana 2,598,481,688 uniq kmers

| | Time | 
|:-|-:|
| MQF | more than 3 hour |
| MPHF | 41 m|
| Brisk | $\emptyset$ |

| | Memory | per kmer | 
|:-|-:|-:|
| MQF | $\emptyset$ | $\emptyset$ |
| MPHF | 49 Gb | 19 bytes |
| Brisk | $\emptyset$ | $\emptyset$ |

