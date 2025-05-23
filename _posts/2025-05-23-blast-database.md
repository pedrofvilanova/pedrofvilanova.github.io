---
title: '[Bioinformatics #1] Using BLAST on the command line'
date: 2025-05-23
permalink: /posts/blast-database/
tags:
  - blast
  - bioinformatics
  - tutorial
---

![image](https://github.com/user-attachments/assets/dfc2898b-1df8-4ccf-a7ab-ef8823cd39b8)


Anyone who has worked with bioinformatics has had to perform at least one BLAST search before. Sometimes we need to deal with large datasets that are beyond the capacity of the NCBI's web platform, which forces us to use the command line. Most people who are starting at bioinformatics don't know that blasting on the command line is easier than it seems. This tutorial aims to describe how to install BLAST, create a local database, and start blasting.

:wrench: Installing BLAST
======

There are multiple ways to install BLAST in your local computer or cloud server. Sometimes I find the easiest way to be through ``conda``:

```
conda install bioconda::blast
```


:computer: Creating a local BLAST database
======

To create a BLAST database, first prepare your ```.fasta``` file, which contains the sequences you want to include in the database. These sequences will be the _subject_ sequences for the BLAST database in which _query_ sequences will serve as input. 

<img width="540" alt="image" src="https://github.com/user-attachments/assets/8a835bd1-8e1d-4671-a0e9-8f3b08cf1bb9" />

Clich [here](https://www.researchgate.net/publication/9089461_A_novel_design_of_whole-genome_microarray_probes_for_Saccharomyces_cerevisiae_which_minimizes_cross-hybridization) to see image source.


The command to create the database is ```makeblastdb```, which is quite straightforward. 

Below you can see what the proper multifasta format is:

1. Each sequence starts with ```>``` as the identifier.
2. After identifiers, there are nucleotide or amino acid sequences that you wish to include in the database. 

```
>FN297865.1 Escherichia coli partial lacZ gene, strain CECT 423
CCGCTGTGGTACACGCTGTGCGACCGCTACGGCCTGTATGTGGTGGATGAAGCCAATATTGAAACCCACG
GCATGGTGCCAATGAATCGTCTGACCGATGATCCGCGCTGGCTACCGGCGATGAGCGAACGCGTAACGCG
AATGGTGCAGCGCGATCGTAATCACCCGAGTGTGATCATCTGGTCGCTGGGGAATGAATCAGGCCACGGC
GCTAATCACGACGCGCTGTATCGCTGGATCAAATCTGTCGATCCTTCCCGCCCGGTGCAGTATGAAGGCG
GCGGAGCCGACACCACGGCCACCGATATTATTTGCCCGATGTACGCGCGCGTGGATGAAGACCAGCCCTT
CCCGGCTGTGCCGAAATGGTCCATCAAAAAATGGCTTTCGCTACCTGGAGAGACGCGCCCGCTGATCCTT
TGCGAATACGCCCACGCGATGGGTAACAGTCTTGGCGGTTTCGCTAAATACTGGCAGGCGTTTCGTCAGT
ATCCCCGTTTACAGGGCGGCTTCGTCTGGGACTGGGTGGATCAGTCGCTGATTAAATATGATGAAAACGG
CAACCCGTGGTCGGCTTACGGCGGTGATTTTGGCGATAC
>GQ925859.1 Escherichia coli strain MTCC 723 LacY (lacY) gene, partial cds
ACCGGTGAACAGGGTACGCGGGTATTTGGCTACGTAACGACAATGGGCGAATTACTTAACGCCTCGATTA
TGTTCTTTGCGCCACTGATCATTAATCGCATCGGTGGGAAAAACGCCCTGCTGCTGGCTGGCACTATTAT
GTCTGTACGTATTATTGGCTCATCGTTCGCCACCTCAGCGCTGGAAGTGGTTATTCTGAAAACGCTGCAT
ATGTTTGAAGTACCGTTCCTGCTGGTGGGCTGCTTTAAATATATTACCAGCCAGTTTGAAGTGCGTTTTT
CAGCG
```

>Bioinformaticians often need to BLAST a few sequences against large databases, such as the non-redundant (nr) BLAST database. In such cases, you can access these up-to-date databases at https://ftp.ncbi.nlm.nih.gov/blast/db/FASTA/. However, be mindful of the file sizes, storage limits, and processing power required for these databases.

After preparing the database file:

1. The ``-in``  parameter states what the input file for the database will be.
2. The ``-dbtype`` parameter defines if the input sequences are nucleotides (ATCG) or protein (amino acids). Choose according to your needs. Use ``nucl`` if you have DNA/RNA sequences or ``prot`` if you have amino acid sequences.
3. The ``-out`` parameter sets the name of the database, which will have the ``.db`` suffix. 

```
makeblastdb -in input.fasta -parse_seqids -dbtype nucl -out input.db
```

:running: Running local BLAST
======

Now that the database is created, let's imagine I want to blast a set of gene sequences (i.e., nucleotides) against my nucleotide database. I can use the command ```blastn``` to do so. 


```
blastn -evalue 1e-10 -db database/input.fasta -query sequences/query.fasta -out blast_out.csv -outfmt 6 -word_size 7
```

1. The ```blastn``` command means I am querying nucleotide sequences against a nucleotide database.
2. The ```-evalue``` parameter, according to BLAST, sets the expect value, which "represents the number of alignments with scores equal to or better than the observed alignment that could be expected to occur by random chance. A smaller E-value indicates a more statistically significant match."
3. The ```-out``` parameter indicates the output file the blast searches will be saved.
4. The ```-outfmt 6``` defines the tabular format of the output. When using the 6 parameter, a table will be outputted with the following columns:


```
qseqid      query or source (gene) sequence id
sseqid      subject or target (reference genome) sequence id
pident      percentage of identical positions
length      alignment length (sequence overlap)
mismatch    number of mismatches
gapopen     number of gap openings
qstart      start of alignment in query
qend        end of alignment in query
sstart      start of alignment in subject
send        end of alignment in subject
evalue      expect value
bitscore    bit score
```

Always remember to respect the BLAST algorithms detailed below:

![image](https://github.com/user-attachments/assets/f56ed43a-d3fb-43ac-84dd-e0925704913a)

Check the tutorial from Oregon State University for more information and images [here](https://open.oregonstate.education/computationalbiology/chapter/command-line-blast/)

Happy blasting! :)
