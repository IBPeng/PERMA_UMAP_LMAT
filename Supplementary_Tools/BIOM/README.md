## BIOM.R

This script/module converts LMAT output in a BIOM* format that is used in the phyloseq 
R package.  Phyloseq is an R package with nice graphical features to analyze microbial census data.

Some examples of the Phyloseq applications can be found here:

[link_to_paper](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0061217)

and the package can be downloaded here: 

[Download](https://bioconductor.org/packages/release/bioc/html/phyloseq.html)

### What it does?
The BIOM tables are the following: OTU_Reads, OTU_RA and Tax_Full.

Lets say that we have a batch of 8 samples, each sample is processed through LMAT for taxonomical identification. Each sample has n1,...,n8 taxonomical hits, where all <a href="https://www.codecogs.com/eqnedit.php?latex=n_j" target="_blank"><img src="https://latex.codecogs.com/png.latex?n_j" title="n_j" /></a> may not be equal. Then to create an even matrix among all 8 samples the dimension of the OTU_READS=(<a href="https://www.codecogs.com/eqnedit.php?latex=max(n_j)" target="_blank"><img src="https://latex.codecogs.com/png.latex?max(n_j)" title="max(n_j)" /></a>,8). For each taxonomic id not found common in other samples then the reads for that are input as 0.

Each column would represent a sample and each row a taxonomical identification labeled by taxid. The OTU_RA works the same but elements in the data frame, are relative abundance of each taxonomical hit per sample. The Tax_Full matrix provides the lineage for each taxid, which is useful to represent the sample per family, genus or other hierarchical level; depending on whats specified in the phyloseq method.


*Input:*
- **concatenated.file** = is all of the *.species* summary outputs per sample of LMAT concatenated together in a single file such that each line has the sample where they belong to.

- **Tax_Ref**= this is a mapping file to obtain the lineage information for a specific taxid based on the reference genomes that are stored in the current LMAT database. [This file is too big to upload, but the script used to create it will be made public]

### Command Line
 ```
 #After having the LMAT results for your input fastas
 
ls -1 \*.species > file_lst

#Merge the files with column showing proceeding filename

cat file_lst|perl g.pl >> concatenated.file

#Merge the Tax_Reference files

cat Tax_Ref* >> Tax_Ref

#Create BIOM tables
$threshold=0 #This variable serves for pruning of reads

RScript BIOM.R concatenated.file $threshold
 ```
*Output:*
All the files are stored in a RData environment as: concatenated.file.RData

Some examples can be seen in the **Example** folder under LMAT-BIOM. Apologies for how some figures look in the pdf version, running the R markdown file will yield nicer graphics than the ones displayed in the pdf.
