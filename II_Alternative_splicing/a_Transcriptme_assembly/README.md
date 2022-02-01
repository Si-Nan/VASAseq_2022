# Overview

To perform transcriptome assembly using VASA-seq data we further extended some workflows that were initally developed for demostrative purposes at Hemberg's group. The [repository](https://github.com/hemberg-lab/RNA_seq_snakepipes/) of these workflows is available on GitHub, under a branch called `joe`.

This and all the computational workflows used alternative splicing analyses were implemented using Snakemake<sup>[1](https://snakemake.readthedocs.io/en/stable/)</sup>, a popular workflow manager that enable the creation of scalable and repoducible computational workflows.  


# Obtaining the code

To obtain the code from our repository use:

```{bash}
git clone https://github.com/hemberg-lab/RNA_seq_snakepipes/
```

Then to access the right branch, you should run:

```{bash}
git checkout joe
```

# Setup

To run these workflows you need to [install Snakemake](https://snakemake.readthedocs.io/en/stable/getting_started/installation.html) and allocate insided the clone folder specific files that will be described bellow.


## config.yaml

This YAML file contains the information that is critical for configuring the pipeline. I suggest you edit our files, here I will list the set of variables you must change to make it run with your data:


- Genome : Here you should include the path to genome (multi-fasta file containing all chromosomes)
- GTF : Path to a GTF file  - Please check that both Genome and GTF have the same chromosome names. For example European names are 1, 2, 3 …while American are Chr1, Chr2, Chr3 …


Other variables that are important:

- samples : samples.tsv
- units : units.tsv
- group_by_cluster : True


But for now we are going to leave them as they are. These variables basically indicate the names of the files will be used by the pipeline and also indicate we want to group cells by cluster during the analysis. There are some variables the pipeline will not use, but I recommend not to delete these lines to just do the minimal amount of modification to the files that are fully working for us.


# References
