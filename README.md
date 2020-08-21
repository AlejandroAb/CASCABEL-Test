# CASCABEL-Test
In this repository you fill find test data for [CASCABEL](https://github.com/AlejandroAb/CASCABEL) and some of it's expected results.
This test data consist of a subset of 100,000 random amplicon sequencing peared reads from water column from Lacamas Lake (WA, US), NCBI's Bioproject [PRJNA524776](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA524776)   
Here, you can also find the configuration and some of the results for the mock community analyses performed for CASCABEL.

## Files in this repo

- **config.summer_winter.yaml** Configuration file used for a multiple library RUN. 
- **winter_summer.dag.png** Directed Acyclic Graph with all the steps to been executedby the pipeeline 
- **libraries.txt** Input file with the reference of the input files for _CASCABEL_.
- **rawdata/** Directory with rawdata
  - **LCSummer_R1.100K.fastq.gz** Forward raw reads from the *"summer"* library.
  - **LCSummer_R2.100K.fastq.gz** Reverse raw reads from the *"summer"* library.
  - **LCWinter_R1.100K.fastq.gz** Forward raw reads from the *"winter"* library.
  - **LCWinter_R2.100K.fastq.gz** Reverse raw reads from the *"winter"* library.
  - **sampleList_mergedBarcodes_summer.txt** Barcode mapping file for demultiplexing the *"summer"* library 
  - **sampleList_mergedBarcodes_winter.txt** Barcode mapping file for demultiplexing the *"winter"* library
- **results/** Directory with some of the main expected results from _CASCABEL_.
  - **otuTable_noSingletons.txt** OTU table with with singletons filtered
  - **report_vsearch.zip** _CASCABEL's_ OTU final report
  - **report_dada2.zip** _CASCABEL's_ ASV final report
  - **representative_seq_set_noSingletons_aligned_pfiltered.fasta.gz** Alignment performed with the representative sequences of the filtered OTU table.
  - **representative_seq_set_tax_assignments.txt** Taxonomy assignation results for the OTU workflow. 
  - **summary.tar.gz** OTU tables summarized at different taxonomy levels (phylum, class, order, famyly, genus and species)
- **MOCK_ANALYSIS/** Directory with configuration files and  _CASCABEL_.

  
## How to use this data

This "mini tutorial" assumes that you already have installed and have a basic idea on how to use _Cascabel_, otherwhise please refer to the official [wiki site of the project](https://github.com/AlejandroAb/CASCABEL/wiki).

In order to use this data for testing purpouse, you only need to download the rawdata directory the reference for inputfiles "libraries.txt" and the configuration files (config.*.yaml).

Once you have downloaded this data the next step is to customize the libraries.txt file accordingly to the path where your inputfiles are located, e.g,:
```
summer  /FULL/PATH/CASCABEL-Test/rawdata/LCSummer_R1.100K.fastq.gz    /FULL/PATH/CASCABEL-Test/rawdata/LCSummer_R2.100K.fastq.gz    /FULL/PATH/CASCABEL-Test/rawdata/sampleList_mergedBarcodes_summer.txt
winter  /FULL/PATH/CASCABEL-Test/rawdata/LCWinter_R1.100K.fastq.gz    /FULL/PATH/CASCABEL-Test/rawdata/LCWinter_R2.100K.fastq.gz    /FULL/PATH/CASCABEL-Test/rawdata/sampleList_mergedBarcodes_winter.txt

```

Other option, is to manually initialize the library structure*. For this, just locate on your CASCABEL directory and execute the following command:
```sass
#init winter library
Scripts/init_sample.sh cascabel_project winter /absolute/path/to/sampleList_mergedBarcodes_winter.txt /absolute/path/to/LCWinter_R1.100K.fastq.gz /absolute/path/to/LCWinter_R2.100K.fastq.gz

#init summer library
Scripts/init_sample.sh cascabel_project summer /absolute/path/to/sampleList_mergedBarcodes_summer.txt /absolute/path/to/LCSummer_R1.100K.fastq.gz /absolute/path/to/LCSummer_R2.100K.fastq.gz
```
_*Please note that you have only to perform this step for executing a multiplex library RUN, in most of the cases you just have to supply the barcode file and raw data information directly into the configuration file_

Now that you have your input files in place, just go to your CASCABEL directory and place there your config.summer_winter.otu.yaml or config.summer_winter.asv.yaml file depending on the analysis you would like to perform. In these files, you just need to update the path to your libraries.txt file, e.g.,:
```yaml
...
...
...
LIBRARY: ["summer","winter"]
...
...
...

#------------------------------------------------------------------------------#
#                             INPUT FILES                                      #
#------------------------------------------------------------------------------#
fw_reads: ""
rv_reads: ""
metadata: ""
input_files: "/FULL/PATH/CASCABEL-Test/libraries.txt"

```
Please notice that the library names "summer" and "winter" matches with the first column of the libraries.txt file.

If you want to generate the DAG (Directed Acyclic Graph) of jobs, as the one supplied on this repo, you can do it with the following command:

```
snakemake --configfile config.summer_winter.otu.yaml --dag | dot -Tpng > winter_sammer.dag.png``
```
To run the pipeline you just need to be located at CASCABEL's directory and execute snakemake:
```
snakemake --configfile config.summer_winter.otu.yaml
```

In the example we are using the configuration file for the OTU workflow, just change to the config.summer_winter.asv.yaml for the ASV workflow.

**Notice** that for the taxonomy assignation within the OTU workflow we are using the mapping files supplied in Cascabel repo at [Cascabel/dbs](https://github.com/AlejandroAb/CASCABEL/tree/master/dbs) so you still need to download the fasta file: [SSURef 138](https://ftp.arb-silva.de/release_132/Exports/SILVA_132_SSURef_Nr99_tax_silva.fasta.gz)  
And for the ASV the [Silva 138 trained database](https://zenodo.org/record/3731176#.XorhSKgzZPY)
