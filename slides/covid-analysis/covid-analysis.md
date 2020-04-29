# COVID Analysis

More precisly correction of covid19 nanopore read and pangenome generation

---

## Project Goal 

Detect variant in covid 19 sequence

----

### Data

Multiplexed nanopore sequencing of reverse transcript covid sample:

- Error rate: ~9%
- Length: ~600 bp
- Potentialy *heterozygote*

----

## Basic method

- medaka
- nanopolish

<p></p>

### IGV DEMO

----

## Trouble

- Inconsistency between the 2 caller variants
- Heterozygosity
- High error rate 
- Variable coverage 

---

## Read correction

We need a correction method to:
- Keep heterozygosity
- High error rate
- Support coverage drop

---

## A thuned GraphAligner correction pipeline

![pipeline](covid-analysis/correction_pipeline.svg)

- kmer-size, begin, end
- reads kmer abundance min

----

## Parameter selection 

![kmer spectrum](covid-analysis/kmer_spectrum.svg)

----

## Parameter selection 

![kmer spectrum](covid-analysis/k19_dbg.svg)

----

## Results

![kmer spectrum](covid-analysis/kmer_spectrum_correct.svg)

----

## Results

Error rate: 

- raw: 6.25%
- k 11: 0.54%
- k 13: 0.367%
- k 15: 0.361%
- k 17: 0.359%
- k 19: 0.364%

Theorical error rate for 20 base variant: 0.05%

---

## Build Pangenome 

Generate a DBG at k = 19 with all kmers with abundance upper than 50 and kmers of reference

### Graphviz demo

----

### Cleaning of pangenome graph

- Remove tips
- Remove homopolymer error
- Remove low covered node
- Your crazy idea I miss

We didn't perform any of this cleaning, generate a pangenome isn't our target.

---

## Call variant

```
Map raw reads of one dataset on pangenome
Concatenate reads they map on same node in a path

Map reference of pangenome

For each path didn't follow the reference:
	Generate sequence of this path
	Generate sequence of the reference
	Create a variant with this two path
```

----

### Result

```
CAAAGCCTCATTATTATTCTTACAAAGTTTATACACCCATTACTTTATGATGCCAACTATTTTCTTTGCTGGCA
||||||||||||||||||||||||||||||||||         |||   | |||||||||||||||||||||||
CAAAGCCTCATTATTATTCTTACAAAGTTTATACGGAACGGCATTTCC-AGGCCAACTATTTTCTTTGCTGGCA
```

Where is the variant ?

---

## Conclusion

- We correct read but we still have some homopolymer error

- How too call variant after pangenome graph mapping

- How too compare variants set from different caller
