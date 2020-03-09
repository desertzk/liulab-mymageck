[![bioconda-badge](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg?style=flat-square)](http://bioconda.github.io)

# MAGeCK: Model-based Analysis of Genome-wide CRISPR-Cas9 Knockout

## Getting MAGeCK 
For a stable version of MAGeCK, please visit:

https://sourceforge.net/projects/mageck/

For the latest MAGeCK documentation, please visit:

https://sourceforge.net/p/mageck/wiki/Home/

MAGeCK is a free, open source software under the BSD license. We greatly appreciate the support from The Claudia Adams Barr Program in Innovative Basic Cancer Research.


## Version history

### New updates since 0.5.9
* 0.5.9.2:Fix a bug to generate excessive number of zeros in mageck count function.
* 0.5.9.1: Fix a bug to incorrectly identify negative control IDS in the paired comparison mode.
* Fix several python2 codes that are not supported by python3.

### Version 0.5.9
* Fix a bug of incorrect sample labels for count table input in mageck count module.
* Revise plotting function.
* Add paired comparisons to compare paired treatment vs control samples.
* Add --variance-estimation-samples option to estimate variances from designated samples; remove --variance-from-all-samples option.
* Add a "-P" option for calculating RRA scores.
* Fix an issue of RRA printing too many debugging information.
* Fix an issue of over-estimating variances from a few guides with extremely high count. Right now, only consider guides whose average count falls below mean(m)+ 4 std(m), where m is the average count of all guides. Thanks Jake Freimer for pointing this out.
* 0.5.8a: Fix a bug to cause math domain error when performing mageck test.

### Version 0.5.8
* Report count table with empty gene IDs.
* Fix a bug to improperly handle the return character in control sgRNA list file in RRA. 
* Fix a bug with improper count of sgRNAs with multiple trim-5 lengths in mageck count command.
* Fix a bug to skip mapped sgRNAs in sam/bam files.
* Improve alpha calculation and gene LFC calculation to exclude guides filtered by --remove-zero option.
* 0.5.7b: Add paired-end read support (Thanks to Wubing Zhang!): find sgRNAs in the 2nd pair if the 1st pair does not include sgRNAs.
* Add warning messages if normalization appears to be improper.
* Mean-var modeling is now based on control samples, unless --variance-from-all-samples is used.
* 0.5.7a: MLE permutation will performed on negative control sgRNAs, if --control-sgrna option is provided.
* Skip MLE calculation for genes whose number of sgRNA is greater than a threshold (defined in --max-sgrnapergene-permutation). This change will greatly increase the speed of MLE calculation.

### Version 0.5.7
* Change the average calculation of sgRNA counts from mean to median to better tolerate outliers.
* Add a beta function to estimate CNV profiles based solely on CRISPR screening data.
* Fix a bug to calculate positive selection lfc values of genes from negative selection sgRNAs.
* Improve the wald p value calculation method in MLE module (Thanks to Chen-Hao!).
* Update the permutation p value calculation of MLE. Genes with the same sgRNAs are permuted together to calculate p values. It takes longer time but the p values are more accurate. Also change the default --permutation-round to 2 to save time.
* Improve variance estimation in RRA by considering raw variance. If more samples are provided, the adjusted variance will also consider the raw variances, calculated directly from these samples. If more than 8 samples are used (control + treatment), the adjusted variance will be calculated directly from raw variances.
* Improve --remove-zero option to allow users choose from none, control, treatment, both or any; set default to both; also allow users to choose the threshold of --remove-zero by adding --remove-zero-threshold option.
* Improve the ranking of sgRNA score to avoid the bias of low read-count sgRNAs in samples with many low read-count sgRNAs. The results of RRA will also be improved based on the revised score.


### Version 0.5.6
* Fix a bug of pathway enrichment analysis
* Add a QC module for count analysis. Use the --day0-label option to enable the QC module and specify the control sample label (usually day 0 or plasmid). For all other sample labels, MAGeCK will invoke "test" command to get negative selection gene list, and invoke "pathway" command to check whether known genes (specified by --gmt-file option) are negatively selected.
* By default, the permutation of RRA is now done by considering genes with the same number of sgRNAs. The speed of RRA is decreased (as permutation is performed separately) but the p value estimation is more accurate.
* By default, MAGeCK count command will automatically determine the trimming length of the fastq file. 
* Add a parameter (--additional-rra-parameters) to allow users more controls of RRA calling.
* RRA now allows filtering genes by their numbers and percentages of good sgRNAs. This will improve the FDR calculation.
* Fix a bug of processing BAM file headers.
* Suppress too many warnings in BAM file processing due to duplicated sgRNA sequences.
* RRA will skip control genes if control sgRNA is specified.
* Incorporate a beta version of CNV normalization.
* Fix a bug of incorrect column number in pathway enrichment analysis.
* Add a parameter (--max-sgrnapergene-permutation) in RRA to increase the speed of RRA when a region is targeted by many sgRNAs.
* Add functions to correct the effects of copy number variation (CNV) in both RRA and MLE modules -- thanks to the great work from Alex Wu.

### Version 0.5.5
* Allow read count table as input run running the count module. This will allow users to normalize counts and get statistics based on count tables.
* Allow BAM and SAM files as input when running the count module. This will allow users to use read mapping algorithms (like Bowtie) to map reads and collect read counts
* Try to match the longest sgRNA first when do the counting
* Fix a bug to require IPython for mle
* Suppress negative control gene output when --control-sgrna is specified
* Fix a bug to cause unexpected exit when calculating sgRNA efficiencies
* For test module, MAGeCK now reports the log fold change (LFC) of gene level using various calculation methods, including mean, median, mean/median for "good" sgRNAs, and "secondbest" that reports the LFC for the second best sgRNA.

### Version 0.5.4 
* Fix a bug in MLE design matrix file processing
* Fix a bug in --unmapped-to-file option
* Update RRA positive selection procedure, and a smaller alpha cutoff is set for stronger positive selection experiments. The results are now less affected by 0-count sgRNAs
* Add an extra step in MLE to skip instances with too many sgRNAs and too many conditions (causes memory error)
* Improve the argument parsing module
* Initial incorporation of Bayes estimation (experimental)
* Improve MLE beta score estimation to avoid biased beta distribution and unreasonably large beta scores for some genes.
* Add multiprocessing options for MLE module.
* Remove the 100,000 gene limit in RRA.
* MAGeCK now doesn't require a password to unzip the source code.
* Disable run subcommand and update demo.

### Version 0.5.3 
* Update functions for pdf-report options
* For count command, now it supports gRNAs with various lengths. The program will automatically extract length information from library file, and the --sgrna-length option will not be working if the library file is specified
* MAGeCK count now supports gzipped fastq files besides fastq files. You can provide the gzipped files (the file name must end with .gz) directly to MAGeCK

### Version 0.5.2

* For variable --trim-5 option, we advise using cutadapt program as the pre-processing step of mageck count
* Improve the --pdf-report plots and statistics of mageck count
* A new (still experimental) maximum-likelihood (MLE) approach for gene calling is introduced 

### Version 0.5.1

* Add one real dataset workflow in the documentation
* MAGeCK can now run on python 3 (still experimental)
* FDR calculation method can be specified on both sgRNA and gene level
* The column header of the output has changed to a more human readable form
* Add several more statistics to read count summary


### Version 0.5

* Add multiple visualization functions
* Fix a bug in negative binomial calculation (thanks to Ido Tamir)

### Version 0.4.4

* Improve the running speed of the RRA program.
* For gene testing, MAGeCK now accepts multiple -t and -c pairs, allowing generating one summary table containing results of multiple comparisons. (However, the integration with pathway analysis is not fully supported.)
* Modify the format of gene_summary.txt; the duplicated "item" column is now removed for positive selection results. Also, two more columns are added to better help users identify true hits: "lo": the RRA lo values; "goodsgrna": the number of sgRNAs in this gene whose ranking is higher than the alpha threshold. 
* MAGeCK now allows users specifying a set of control sgRNAs to generate null distributions.
* Fix two bugs in calculating the median factor during normalization (thanks to Bastiaan Evers).
* Add the "-v/--version" command.

### Version 0.4.3

* Fix a bug where the program exits unexpectedly for certain samples with many 0 read counts.
* Fix a bug of pathway analysis where the RRA program stops early for certain gene belonging to too many pathways.

### Version 0.4.2

* Create Youtube tutorial videos for installation and sample comparisons.
* Improve the median normalization method to handle cases with many zero-count sgRNAs.
* The median normalized read count are provided in the count command.
* Modify the count command line options to accept combining reads from technical replicates.
* Provide simple statistics for processing fastq files.
* Provide library file for Synergistic Activation Mediators (SAM), a CRISPR activation protocol developed in Feng Zhang laboratory (http://www.addgene.org/crispr/libraries/sam/).

### Version 0.4.1

* Increase the default alpha cutoff from 0.05 to 0.25.
* Provide some of the commonly used library files for the convenience of users.

### Version 0.4

* Added the BSD license information.
* Improved the logging system.
* The control_id and treatment_id options now can be specified using sample strings.
* Merge positive selection and negative selection genes and pathways into one file.
* Add the --keep-tmp option to control intermediate files after running.
* Fixed one bug in FDR calculation.

### 2014.07.01 Version 0.3

* The installation method is changed so users can now more easily install the software.
* Added a new feature to detect enriched pathways (pathway command)
* Changed the input format of the program. The second column of the count table (generated by the count subcommand and used by the test subcommand) is now the gene name.
* For the count subcommand, the sgRNA information is provided with the library file.

### 2014.04.17 Version 0.2

* Updated the demo and wiki page

### 2014.04.04 Version 0.1

* The source code released.
