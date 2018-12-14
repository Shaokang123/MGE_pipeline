# MGE_pipeline alpha-test version
# Introduction
This is a pipeline for the detection of mobile genetic elements (MGEs) from complete genomes or genome assemblies. The types of detected MGEs include prophage, insertion sequence (IS), integron and integrative and conjugative elements (ICEs). This pipeline integrates four tools for the detection of each MGE type into a single workflow. Prophages are detected using PHASTER's URLAPI, insertion sequences are detected using ISEScan, integrons are detected using Integron Finder and ICEs are detected using CONJScan.

# Dependencies
1. Python 2.7 and Python 3.4
2. Python modules: biopython, pandas and numpy
3. NCBI BLAST v2.7.1+
4. [Prokka](https://github.com/tseemann/prokka) v1.12 
5. [ISEScan](https://github.com/xiezhq/ISEScan) v1.6
6. [macsyfinder](https://github.com/gem-pasteur/macsyfinder) v1.0.4 and [CONJScan module](https://github.com/gem-pasteur/Macsyfinder_models/tree/master/models/Conjugation)
7. [Integron_Finder](https://github.com/gem-pasteur/Integron_Finder) v2.0
8. [PHASTER](http://phaster.ca/)'s URLAPI
9. [Vsearch](https://github.com/torognes/vsearch) v2.8.1

# Instructions:
The Executable scritps of NCBI BLAST, macsyfinder and Integron Finder are included in this repository, but the following programs and their required dependencies should be installed prior to the execution of the MGE pipeline.
1. Make sure python2.7, python3.4 and the required modules are installed.
2. Install Prokka. 
3. Install ISEScan. Make sure an executable "isescan.py" is added to your PATH.
4. Install vsearch.
5. Run the MGE pipeline using the following command.

# Usage
```
usage: mges_pipeline.py -i <assembily_path> -o <output_path> -t <threads> -c <minimum_coverage> -p <minimum_identity>

optional arguments:
  -h, --help:  show this help message and exit
  -i I <string>: input assemblies
  -o O <string>: output directory
  -t T <string>: threads
  -c C <int>: minimum coverage of each hit for MGE verification using BLASTn, default 90.
  -p P <int>: minimum identical percentage of each hit for MGE verification using BLASTn, default 90.
```
# Output files
#### 1. mge_matrix.csv
A binary matrix that summarizes the presence/absence of MGEs in each of the detected genomes.

| Sample | prophage_1 | prophage_2 | insertion_1 | insertion_2 |
| :----- | :----- | :----- | :----- | :----- |
| SRR5336039 | 1 | 1 | 1 | 1 |
| SRR5852955 | 1 | 1 | 0 | 1 |
| SRR5932646 | 1 | 1 | 0 | 1 |

#### 2. mge_length.csv
A table that summarizes the total length of each type of MGEs.

| Sample | prophage | insertion | ice | integron | total |
| :----- | :----- | :----- | :----- | :----- | :----- |
| SRR5336039 | 73804 | 18227 | 0 | 0 | 92031 |
| SRR5852955 | 73804 | 14101 | 0 | 0 | 87905 |
| SRR5932646 | 67484 | 11988 | 0 | 0 | 79472 |

#### 3. mge_dist_matrix.csv
A matrix that summarizes the number of different MGEs in each pair of tested genomes.

| 	 | SRR5336039 | SRR5852955 | SRR5932646 |
| :----- | :----- | :----- | :----- |
| SRR5336039 | 0 | 1 | 2 |
| SRR5852955 | 1 | 0 | 1 |
| SRR5932646 | 2 | 1 | 0 |

#### 4. MGE_gff and MGE_sequences fasta files
Each gff file will list all the detected MGE sequences in each genome according to the gff format. The MGE_sequence fasta file put all the detected sequences of each MGE type together.
```
SRR4176676_19	Phaster	prophage	37543	87358	.	.	.	cluster_id "prophage_1"; length "49816"
SRR4176676_27	Phaster	prophage	14657	49480	.	.	.	cluster_id "prophage_2"; length "34824"
SRR4176676_2	ISEScan	insertion	362655	364769	.	.	.	cluster_id "insertion_2"; length "2115"
SRR4176676_10	ISEScan	insertion	503	2116	.	.	.	cluster_id "insertion_5"; length "1614"
SRR4176676_24	Blast	insertion	54451	55511	.	.	.	cluster_id "insertion_16"; length "1061"
SRR4176676_23	CONJscan	ice	10901	43497	.	.	.	cluster_id "ice_2"; length "32597"
SRR4176676_42	Integron finder	integron	1930	6319	.	.	.	cluster_id "integron_1"; length "4390"
```
# Reference:
1. Seemann, T. (2014). Prokka: rapid prokaryotic genome annotation. Bioinformatics, 30(14), 2068-2069.
2. Arndt, D., Grant, J. R., Marcu, A., Sajed, T., Pon, A., Liang, Y., & Wishart, D. S. (2016). PHASTER: a better, faster version of the PHAST phage search tool. Nucleic acids research, 44(W1), W16-W21.
3. Xie, Z., & Tang, H. (2017). ISEScan: automated identification of insertion sequence elements in prokaryotic genomes. Bioinformatics, 33(21), 3340-3347.
4. Cury, J., Jové, T., Touchon, M., Néron, B., & Rocha, E. P. (2016). Identification and analysis of integrons and cassette arrays in bacterial genomes. Nucleic acids research, 44(10), 4539-4550.
5. Abby, S. S., Cury, J., Guglielmini, J., Néron, B., Touchon, M., & Rocha, E. P. (2016). Identification of protein secretion systems in bacterial genomes. Scientific reports, 6, 23080.

