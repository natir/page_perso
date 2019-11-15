## Novel components at the periphery of long read genome assembly tools

Pierre Marijon

2 Decembre 2019

---

## Genome Assembly Trouble

----

### What genome assembly is ?


![](thesis/images/DNA.svg)

|Generation | Technology          | Read length (bd)                 | Error rate |
|:- |:- | -:| -:|
First      | Sanger              | $\approx$ 2 kb                   | Low ($\approx$ 2\%) |
Second     | ABI/Solid           | 75                               | Low ($\approx$ 2\%) |
Second     | Illumina/Solexa     | 100–150                          | Low ($<$2\%) |
Second     | IonTorrent          | $\approx$ 200                    | Medium ($\approx$ 4\%) |
Second     | Roche/454           | 400–600                          | Medium ($\approx$ 4\%) |
Third      | Pacific Biosciences | $\approx$ 10 kb ($\max$ 100 kb)  | High ($\approx$ 18\%) |
Third      | Oxford Nanopore     | $\approx$ 10 kb ($\max$ 1 mb)    | High ($\approx$ 12\%) |

Note:

- La 'vie' stock l'information necessaire a sa survie, reproduction, évolution
- Généralement stocké sur l'ADN

- On dispose de séquenceur pour lire cette ADN
- Différente longueur différent taux d'erreur

- Ces fragment sont intéressant mais les génomes sont très grand

----

### Trouble definition

Crazy monk analogie

![](thesis/images/assembly_pipeline.svg)

Note:

Imaginé vous été un aventurier en quête d'un livre magique.
Ce livre est dans les mains d'un moines copiste qui refuse de vous le donnée mais qui recopie des fragments

Sauf que le moines est fou et il commence a recopié ou il veux il fait des erreurs parfois il écrits n'importe quoi et le livre parfois ce répéte

Le live c'est le génome, le moine c'est le séquenceur l'assemblage c'est reconstruire le livre d'origine

----

### Greedy algorithm

----

### Debruijn Graph algorithm

----

### Overlap Layout consensus algorithm

---


## Pre Assembly

----

### Error filtering

----

### Overlap detection

----

#### Overlap management

---

## Post Assembly

----

### Assembly isn't solved


---


### Conclusion
