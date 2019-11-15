# PCON BR

Prompt CouNter, Brutal Rewrite

In collaboration with Antoine Limasset

---

## Project Goal 

Perform self or hybrid precorrection of long read.

---

## PCON Method

Pcon perform full ram kmer counting

----

## With some limitation

- count range between 0 to $2^n$ $n \in (4, 8, 16)$
- k must be odd
- memory usage grow exponentialy with k: 
	$\frac{2^{2k - 1}}{\frac{8}{n}}$ bytes
- count only canonical kmer
```python
def get_cannnonical(kmer):
	reverse = revcomp(kmer)
	if kmer < reverse:
		return kmer
	else:
		return reverse
```
- other caracter than A C T G was convert silently in A C T or G

----

### How it's work

Binary conversion of nucleotide:

| nucleotide | ascii value | bin value | 2 bit |
|:- | -:| - | -:|
| A | 65 | 01000**00**1 | 00 |
| C | 67 | 01000**01**1 | 01 |
| G | 71 | 01000**11**1 | 11 |
| T | 84 | 01010**10**0 | 10 |
| N | 78 | 01001**11**0 | 11 |

----

### How it's work

- Complement: `complement kmer = kmer ^ 0b010101…`
- Reverse: a modified lookup table reverse bit algorithm
- Get cannonical version of a kmer:
```python
def get_cannonical(kmer):
	if popcount(kmer) % 2 == 0:
		return kmer
	else:
		return revcomp(kmer)
```

----

### An example

Sequence: ACTGGAA 

k: 5

| kmer | 2bit | popcount | canonical |
|:- | - | - | -:|
| ACTGG | 0001101111 | 6 | 0001101111 |
| CTGGA | 0110111100 | 6 | 0110111100 |
| TGGAA | 1011110000 | 5 | 1010010100 |

----

### Algorithm overview

```python
count_table = [0] * pow(2, 2 * k - 1)

for sequence in file:
	for kmer in sequence:
		cannonical = get_cannonical(kmer)
		hash = cannonical >> 1
		count_table[hash]++
```

Note:
We can reverse hash easily

---

## Pcon evaluation

----

### Dataset

| code name       | species          | # bases (Gb)| coverage |
|:----------------|:-----------------|------------:|---------:|
| s_pneumoniae    | S. pneumoniae    |         2.2 |   ~1000x |
| c_vartiovaarae  | C. vartiovaarae  |         1.7 |    ~150x |
| e_coli_ont      | E. coli          |         1.6 |    ~340x |
| e_coli_pb       | E. coli          |         1.4 |    ~297x |
| s_cerevisiae    | S. cerevisiae    |       0.187 |     ~15x |

----

### Performance

![We are best than other one](pconbr/memory.svg)

Memory in mb

----

### Performance

![We are best than other one](pconbr/timming.svg)

Time in seconds

----

### Point of improvement: Cache locality

Sequence: ACTGGAA

k: 5

| kmer | hash |
|:- | -:|
| ACTGG | 55 |
| CTGGA | 222 |
| TGGAA | 376 |

We want a new data structure with some property:

1. successive kmer was store near in memory
3. good acces time (O(1) or not too fare)
3. does not (too much) increase the memory costs


Note:
For this value of k we stay et cpu cache with larger value of k we invalidate cache many time

---

## BR method

<style>
.reveal pre code {
max-height: None;
}
</style>

```
solid = load pcon abundante more than a threshold

for sequence in file:
	for kmer in sequence:
		if kmer in solid:
			continue
		else:
			error_type = list()
			alternative_kmer = get_alternative_kmer(solid, kmer)
			
			if more than one alternative_kmer:
				continue
			
			if alternative_kmer work in substitution case on s next kmer:
				error_type.append("Sub")
			if alternative_kmer work in insertion case on s next kmer :
				error_type.append("Ins")
			if alternative_kmer work in deletion case on s next kmer:
				error_type.append("Del")
				
			if len(error_type_kmer) > 1:
				continue
			else:
				correct_sequence()
```

----

## BR method

Three parameter:

- k: size of kmer
- a: minimal abundance of kmer to consider is a correct kmer
- s: number of kmer need to be correct after a modification to validate it

---

## BR evaluation

----

### Dataset

Same dataset:

| species         | # bases (Gb) | coverage | mean error rate |
|:----------------|-------------:|---------:|-----------------|
| S. pneumoniae   |          2.2 |   ~1000x |          14.2 % |
| E. coli ONT     |          1.6 |    ~340x |          22.8 % |
| E. coli PB      |          1.4 |    ~297x |          19.4 % |

Synthetic dataset [Badread](https://github.com/rrwick/Badread) on *E. coli* CFT073 reference genome:

| mean targeted error rate | coverage | mean real error rate |
|:----------------:|---------:|-----------------|
| 15 % |     100x |           |
| 10 % |     100x |          10.66 % |
| 5 % |     100x |          5.29 % |

----

### Best result for each dataset

| dataset | k | a | s | error rate reduction |
|:- | -:| -:| -:| -:|
| S. pneumoniae | 17 | 15 | 3 | -0.56904 |
| E. coli ONT | 19 | 15 | 1 | -1.58514 |
| E. coli PB | 17 | 15 | 1 | -2.12523 |
| Synthetic 85 | | | | |
| Synthetic 90 | 17 | 11 | 1 | -3.186835 |
| Synthetic 95 | 17 | 15 | 1 | -2.254555 |


----


### Kmer spectrum

---

## What remains to be done

![](pconbr/final_meme.png)

- automaticly detect good abundance value (maybe same stuff for k)
- test multiple iteration of pconbr (or only br) with differente value of k
- test on 90 % identity real dataset
- test hybrid correction

