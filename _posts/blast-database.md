---
title: 'Creating a BLAST database'
date: 2025-05-18
permalink: /posts/2025/05/blast-database/
tags:
  - blast
  - bioinformatics
  - tutorial
---

Anyone who has worked with bioinformatics has had to perform at least one BLAST search. Sometimes we need to deal with large datasets that are beyond the capacity of the NCBI's web platform, which forces us to use the command line. Most people who are starting at bioinformatics don't know that blasting on the command line is easier than it seems. This quick tutorial aims at elucidating that. 

Creating a local BLAST database
======

To create a BLAST database, first prepare your ```.fasta``` file containing the sequences you wish compose the database, i.e, they will be the sequences that our input sequences to blast will be blasted against. The command to create the database is ```makeblastdb``` which is pretty intuitive. 

1. The ``-in``  parameter states what the input file for the database will be.
2. The ``-dbtype`` parameter defines if the input sequences are nucleotides (ATCG) or protein (amino acids). Choose according to your needs. Use ``nucl`` if you have DNA/RNA sequences or ``prot`` if you have amino acid sequences.
3. The ``-out`` parameter sets the name of the database, which will have the ``.db`` suffix. 

```
makeblastdb -in input.fasta -parse_seqids -dbtype nucl -out input.db
```

Running local BLAST
======

Now that the DB is created, let's imagine I intend to blast a set of gene sequences (i.e, nucleotides) against my nucleotide database. I can use command ```blastn``` to do so. 

```
blastn -query query.fasta -db input.fasta -out output.tsv
```

