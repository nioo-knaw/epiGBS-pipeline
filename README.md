Contents
========

* [How to use this file](#how-to-use-this-file)
* [Introduction](#introduction)
* [Login on the server](#login-on-the-server)
* [Copying the pipeline](#copying-the-pipeline)
* [Directories](#directories)
* [Prerequisites](#prerequisites)
* [Start the pipeline](#start-the-pipeline)
* [More Reading](#more-reading)


How to use this file
---------------------

This README file gives a global introduction on how to work on the server, cloning the pipeline repository and how to run the pipeline. We tried to be as complete and precise as possible.
If you have any question, comment, complain or suggestion or if you encounter any conflicts or errors in this document or the pipeline, please contact your Bioinformatics Unit (Bioinformatics-support@nioo.knaw.nl) or open an `Issue`!

###### Enjoy your analysis and happy results!

```
Text written in boxes is code, which usually can be executed in your Linux terminal. You can just copy/paste it.
Sometimes it is "special" code for R or any other language. If this is the case, it will be explicitly mentioned in the instructions.
```

`Text in a red box indicates directory or file names`

Text in brackets "<>" indicates that you have to replace it and the brackets by your own appropiate text.

Introduction
------------

epiGBSia a reduced representation bisulfite sequencing method for cost-effective exploration and comparative analysis of DNA methylation and genetic variation in hundreds of samples de novo. This method uses genotyping by sequencing of bisulfite-converted DNA followed by reliable de novo reference construction, mapping, variant calling, and distinction of single-nucleotide polymorphisms (SNPs) versus methylation variation.

This pipeline is cloned from https://github.com/thomasvangurp/epiGBS and adapted to run in a conda environment on the NIOO-bioinformatics servers.

The original reference is accessible [here](https://www.nature.com/articles/nmeth.3763).


Login on the server
------------------

If you will login to the bioinformatics server for the first time, please contact the BU or refer to the tutorial on gitlab (https://gitlab.bioinf.nioo.knaw.nl/tutorials/server-login) or download the corresponding .pdf from the left hand site on the intranet (https://intranet.nioo.knaw.nl/en/bioinformatics-unit).

Copying the pipeline
------------------

To start a new analysis project based on this pipeline, follow the following steps:

- Clone and rename the pipeline-skeleton from our GitLab server by typing in the terminal. Replace <name> by your NIOO login-name. Cloning will only work, if you have logged in to gitlab at least once before:

```
git clone https://gitlab.bioinf.nioo.knaw.nl/pipelines/epiGBS.git
```

- Enter `epiGBS` and download epiGBS code

```
cd epiGBS
git submodule init
git submodule update
```

Directories
--------------------------

##### The toplevel `README` file

This file contains this content with general information about how to run this pipeline.

##### The `data` directory

Place your epiGBS read files here.

##### The `src` directory

Contains the code of the epiGBS pipeline.

##### The `docs` directory

Here, you can store all other files, like metadata etc.

##### The `analysis` directory

This is the directory, where you will analyse your data and all output will be generated.

Prerequisites
------------------

You have to install different dependencies using conda before starting the pipeline for the first time:

```
conda env create -f environment.yaml
source activate epiGBS
```

if you already have created the environment `epiGBS` before, than activate it by

```
source activate epiGBS
```

You can deactivate the environment after pipeline execution with

```
source deactivate
```

Start the pipeline
------------------

##### Activate the epiGBS environment

(see [Prerequisites](#prerequisites) for detailed instructions).
```
source activate epiGBS
```

##### Add read files in the `data` directory.

see [Directories](#directories)

##### Make a barcode file and add to the `data` directory

The barcode file is tab-delimited and contains at least the following columns: `Flowcell`, `Lane`, `Barcode_R1`, `Barcode_R2`, `Barcode_R2`, `Sample`, `ENZ_R1`, `ENZ_R2`, `Wobble_R1`, `Wobble_R2`. All other fields are mandatory.

```bash
# barcodes.tsv
Flowcell        Lane    Barcode_R1      Barcode_R2      Sample  history Country PlateName       Row     Column  ENZ_R1  ENZ_R2  Wobble_R1       Wobble_R2       Species
H53KHCCXY       5       AACT    CCAG    BUXTON_178      C       BUXTON  BUXTON_WUR_AseI_NsiI_final_run1 1       2       AseI    NsiI    3       3       Scabiosa columbaria
H53KHCCXY       5       CCTA    CCAG    WUR_178 C       WUR     BUXTON_WUR_AseI_NsiI_final_run1 2       2       AseI    NsiI    3       3       Scabiosa columbaria
H53KHCCXY       5       TTAC    CCAG    BUXTON_169      C       BUXTON  BUXTON_WUR_AseI_NsiI_final_run1 3       2       AseI    NsiI    3       3       Scabiosa columbaria
H53KHCCXY       5       AGGC    CCAG    WUR_169 C       WUR     BUXTON_WUR_AseI_NsiI_final_run1 4       2       AseI    NsiI    3       3       Scabiosa columbaria
H53KHCCXY       5       GAAGA   CCAG    BUXTON_175      SD      BUXTON  BUXTON_WUR_AseI_NsiI_final_run1 5       2       AseI    NsiI    3       3       Scabiosa columbaria
H53KHCCXY       5       CCTTC   CCAG    WUR_175 SD      WUR     BUXTON_WUR_AseI_NsiI_final_run1 6       2       AseI    NsiI    3       3       Scabiosa columbaria
```

##### Execute the pipeline

Execute the following commands:

```bash
# make some links to python files
bash links.sh

# change directory to analysis
cd analysis

# open the demultiplex.sh script
nano demultiplex.sh
# adjust the --r1, --r2, --barcodes flags accordingle to your input names. Choose a name for --output_dir. Close nano with Ctrl+x
# execute the bash script
bash demultiplex.sh

# if you have a large fastq input file, demultiplexing will take long. Use the Snakefile instead.

nano make_reference.sh
# adjust all paths accordingly to your choices from the previous step
bash make_reference.sh

nano mapping_variant_calling.sh
# adjust all paths accordingly to your choices from the previous step, except path to --tmpdir
bash mapping_variant_calling.sh
```

#### Test data

You can access, copy or link test data from /data/tutorials/epiGBS/test_data/ and run the pipeline.

More Reading
------------------

[Nature methods](https://www.nature.com/articles/nmeth.3763)
