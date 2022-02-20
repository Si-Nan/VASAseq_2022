# VASAseq Analysis Pipeline

The same pipeline was used to analyze the data from (Pijuan-Sala et al, Nature 2018)[https://www.nature.com/articles/s41586-019-0933-9], changing only the relevant parameters in the filtering steps. 

## Contents

### Scripts
- `01_qc_checks_HPCversion.py`: In this notebook we perform the quality checks (QC) of the VASA libraries (mouse embryo data: E6.5, E7.5, E8.5 and E9.5) and identify doublet cells with `scrublet`. The notebook takes as an input the merged tables (in feather format) for both counts obtained using reads mapping in the whole gene bodies, or counts obtained from reads that fall into the 80% side of the 3' end of the gene. 

- `02_scanpy_QCxBiotype.py`: Creates 3 different scanpy objects: 1 for only spliced counts, another for only unspliced counts, and the 3rd for both spliced and unspliced counts. In the 3rd object, spliced and unspliced counts from the same gene are given different gene identifiers. Each scanpy object is further split into new objects, in which only counts from different biotypes are used (protein-coding, smallRNA, lncRNA, tRNA). For all objects, only genes present in at least 2 cells are kept. Extra QC plots showing histone content or fraction of biotypes per cell are produced. 

- `03_cellCycle.py`: Here, we identify cells in S-phase using histone content for all timepoints, separatedly and combined, based on histone expression (using only spliced read counts). We use only count tables produced with all gene counts (falling into any position along the gene body).  We further filter cells based on the parameters in `filterParams.py`. The script performs differential gene expression analysis between S-phase and non-S-phase cells, for both spliced and unspliced counts (separately) using the `scanpy` function.

- `04_scanpy_Filtered_suUMAP_Hist.py`
- `05_PJvsVASA.py`
- `05_labelTransfer_Silhouette_Dex.py`
- `06_masterUMAP.py`
- `06_masterUMAP_noHist.py`
- `07_RNAvelocity.py`

### In house python libraries
- `filterParams.py`: For each timepoint, this contains the parameters used to filter out cells with not enough sequencing quality, the number of PCA used to build the kNN graph, the resolution set to perform leiden clustering, and the dispersion selected to perform UMAP projection. These parameters are common for all libraries in each timepoint. In E8.5, one full library is excluded. As an output, several QC plots are generated. 
- `plot_aautils.py`: Collection of functions used a lot to generate graphs with our cherry-picked properties. 
- `sc_aautils.py`: Collection of functions used recurrently to analyze single cell data and that are not included in the `scanpy` package or that are wrappers of functions that are indeed included in `scanpy`.

### Input parameters
- `HistoneGenes.tsv`: list of histone genes
- `Mus_musculus_TF.txt`: List of annotated murine transcription factors.

## Detailed description of the pipeline

All the scripts can be run direclty in a LINUX/UNIX terminal using a virtual environment with python3 and all the required libraries (`pandas, numpy, collections, scanpy, scrublet, ...`) or submitting the jobs via SLURM or SGE. In some cases, a lot of memory is needed and the script takes some time, therefore job submission is recomended. 

### Examples

- `timepoint` indicates which timepoint are we analyzing and as an input of our scripts we set it as: E65, E75, E85, E95
- `genecoverage` indicates whether we use all reads or only reads mapping the the 3' UTR of detected genes.
- 
```
timepoint=E65
genecoverage=all
01_qc_checks_HPCversion.py $timepoint 
02_scanpy_QCxBiotype.py $timepoint $genecoverage
03_cellCycle.py
```

