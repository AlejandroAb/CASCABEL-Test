# CASCABEL-Test
In this repository you fill find test data for [CASCABEL](https://github.com/AlejandroAb/CASCABEL) and some of it's expected results.
This test data consist of amplicon sequencing reads from water column from Lacamas Lake (WA, US), NCBI's Bioproject [PRJNA524776](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA524776)   

## Files in this repo

- **config.summer_winter.yaml** Configuration file used for a multiple library RUN. 
- **winter_summer.dag.png** Directed Acyclic Graph with all the steps to been executedby the pipeeline 
- **rawdata/** Directory with rawdata
  - **LCSummer_R1.100K.fastq.gz** Forward raw reads from the *"summer"* library.
  - **LCSummer_R2.100K.fastq.gz** Reverse raw reads from the *"summer"* library.
  - **LCWinter_R1.100K.fastq.gz** Forward raw reads from the *"winter"* library.
  - **LCWinter_R2.100K.fastq.gz** Reverse raw reads from the *"winter"* library.
  - **sampleList_mergedBarcodes_summer.txt** Barcode data for demultiplexing the "*summer"* library 
  - **sampleList_mergedBarcodes_winter.txt** Barcode data for demultiplexing the "*winter"* library 
- **results/** Directory with some of the main expected results from _CASCABEL_.
  - **otuTable_noSingletons.txt** OTU table with with singletons filtered
  - **report_vsearch.zip** _CASCABEL's_ final report
  - **representative_seq_set_noSingletons_aligned_pfiltered.fasta.gz** Alignment performed with the representative sequences of the filtered OTU table.
  - **representative_seq_set_tax_assignments.txt** Taxonomy assignation results
  - **summary.tar.gz** OTU tables summarized at different taxonomy levels (phylum, class, order, famyly, genus and species)
  
## How to use this data

This "mini tutorial" assumes that you already have installed and have a basic idea on how to use _Cascabel_, otherwhise please refer to the official [wiki site of the project](https://github.com/AlejandroAb/CASCABEL/wiki).

In order to use this data for testing purpouse, you only need to download the rawdata directory and the configuration file.

Onece you have downloaded this data the next step is to initialize the library structure*. For this, just locate on your CASCABEL directory and execute the following command:
```sass
#init winter library
Scripts/init_sample.sh cascabel_project winter /absolute/path/to/sampleList_mergedBarcodes_winter.txt /absolute/path/to/LCWinter_R1.100K.fastq.gz /absolute/path/to/LCWinter_R2.100K.fastq.gz

#init summer library
Scripts/init_sample.sh cascabel_project winter /absolute/path/to/sampleList_mergedBarcodes_summer.txt /absolute/path/to/LCSummer_R1.100K.fastq.gz /absolute/path/to/LCSummer_R2.100K.fastq.gz
```
_*Please note that you have only to perform this step for executing a multiple library RUN, in most of the cases you just have to supply the barcode file and raw data information directly into the configuration file_

If you want to generate the DAG (Directed Acyclic Graph) of jobs, as the one supplied on this repo, you can do it with the following command:

```
snakemake --configfile config.summer_winter.yaml --dag | dot -Tpng > winter_sammer.dag.png``
```
To run the pipeline you just need to be located at CASCABEL's directory and execute snakemake:
```
snakemake --configfile config.summer_winter.yaml
```
