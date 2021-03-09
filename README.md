# eeb5300_tyler_ashley_project
# This is a test readme commit for a project in eeb5300. This repository will store 4 RNAseq datasets to be analyzed for transcripts of G2/Jockey-3 in somatic (brain) and germline (male and female) tissues of Drosophila Melanogaster

# We will be getting our data from the paper, " Re-annotation of eight Drosophila genomes" by Yang et.al. 2018
# Yang et.al. 2018 extracted RNA from 584 samples from nine different fly lines. RNA was then sequenced using the Illumina HiSeq2000 platform generating approximately 5 billion reads of  average length is 76 bp. We will be using the single-end sequencing data from the male and female gonads/brains of four to 14 day old Drososophila Melanogaster flies. 

# The accession numbers for the data from the Sequencing Read Archive at NCBI are:

# Male brain: GSM2647281 GSM2647282 GSM2647283
# Female brain: GSM2647278 GSM2647279 GSM2647280
# Male germline: GSM2647275 GSM2647276  GSM2647277
# Female germline: GSM2647272 GSM2647273 GSM2647274

# Outline(based on  Model- based RNA seq analysis tutorial by UCONN'S CBC) :
## 1. Clone the RNA seq tutorial using the git clone command and upload our data to the cluster using sratoolkit
## 2. Quality control using Trimmomatic and estimation of quality and coverage using FASTQC.
## 3.  Align reads using HISAT2 and sort/read data using samtools
### We will be using an annotated reference genome from the Mellone Lab at UCONN which is an improvement on the current DM6 genome with replicative satellite and transposon sequences. 
## 4.  Use htseq-count to determine how many reads map to G2/Jockey-3 in the genome for each sample tissue
## 5. Visualize data using a MA and volcano plot and prinicipal component analysis (PCA)-check? 


# Reference: Yang, Haiwang et al. “Re-annotation of eight Drosophila genomes.” Life science alliance vol. 1,6 e201800156. 24 Dec. 2018, doi:10.26508/lsa.201800156
# RNA Seq tutorial : https://github.com/CBC-UCONN/RNA-seq-with-reference-genome-and-annotation
