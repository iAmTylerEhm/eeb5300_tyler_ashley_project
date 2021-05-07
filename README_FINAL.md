Overview and Motivation

Chang et al. 2019 discovered that all centromeres in Drosophila melanogaster are comprised of islands of retrotransposons flanked by simple satellite repeats. Retrotransposons are selfish genetic elements that replicate in the genome by using an RNA intermediate. These elements can disrupt protein-coding sequences and rearrange the genome when replicating (Klein et al. 2018). Though each centromere differed in terms of elements present, G2/Jockey-3 was the only retrotransposon enriched at all the centromeres (Chang et al. 2019). This data begs the question, does G2/Jockey-3 have a role in centromere formation or function? To answer this question, we first need to determine whether G2/Jockey-3 is expressed in the cell.  We will use RNA seq data from the Yang lab to investigate whether G2/Jockey-3 is expressed in male and female brains/gonads. 

Related Work/Citations:
Yang et.al. 2018 extracted RNA from 584 samples from nine different fly lines. RNA was then sequenced using the Illumina HiSeq2000 platform generating approximately 5 billion reads of average length is 76 bp. We will be using the single-end sequencing data from the male and female gonads/brains of 4 to 14 day old Drosophila Melanogaster flies. We will be mapping these reads to the reference genome generated by the Mellone Lab at UCONN. This reference  has the DNA sequences found at the centromere. They performed four replicate ChIPs for the centromeric protein CENP-A in embryos and sequenced the reads using a combination of long-read sequencing and Illumina paired-end sequencing( Chang et al. 2019).  They then used de novo assembling techniques to assemble contigs. 
 
Initial Questions: First we investigated which genes were differentially expressed in male and female brains/gonads. Then we determined if G2/Jockey-3 was expressed in these tissues and whether this expression came from the centromere. 

Data:All data is within the final_project folder. The relevant folders for the project are highlighted below:

Raw data - RNA seq reads from the SRA 
Quality control- Trimmed files and shell script for quality control
FastQC- FastQC and MutliQC files for each trimmed RNA seq replicate
Align- BAM files generated from aligning RNA seq reads to reference genome
We adapted code from UCONN’s RNA seq tutorial (https://github.com/CBC-UCONN/RNA-seq-with-reference-genome-and-annotation)
 
Workflow/Analysis:
Clone the RNA seq tutorial using the git clone command into the final_project directory
```
git clone  https://github.com/CBC-UCONN/RNA-seq-with-reference-genome-and-annotation final_project
```
Upload RNA Seq Data to the cluster using sratoolkit. The script can be found in the raw_data folder
```
#SBATCH --job-name=fastq_dump_xanadu
#SBATCH -n 1
#SBATCH -N 1
#SBATCH -c 1
#SBATCH --mem=15G
#SBATCH --partition=general
#SBATCH --qos=general
#SBATCH --mail-type=ALL
#SBATCH --mail-user=name@uconn.edu
#SBATCH -o %x_%j.out
#SBATCH -e %x_%j.err
echo `hostname`

#################################################################
# Download fastq files from SRA
#################################################################
module load sratoolkit/2.8.2

fastq-dump --gzip SRR5639581
fastq-dump --gzip SRR5639582
fastq-dump --gzip SRR5639583
fastq-dump --gzip SRR5639584
fastq-dump --gzip SRR5639585
fastq-dump --gzip SRR5639586
fastq-dump --gzip SRR5639587
fastq-dump --gzip SRR5639588
fastq-dump --gzip SRR5639589
fastq-dump --gzip SRR5639590
fastq-dump --gzip SRR5639591
fastq-dump --gzip SRR5639592

#################################################################
# Rename the files
#################################################################
mv SRR5639590.fastq.gz maleb_SRR5639590.fastq.gz
mv SRR5639591.fastq.gz maleb_SRR5639591.fastq.gz
mv SRR5639592.fastq.gz maleb_SRR5639592.fastq.gz
mv SRR5639587.fastq.gz fmaleb_SRR5639587.fastq.gz
mv SRR5639588.fastq.gz fmaleb_SRR5639588.fastq.gz
mv SRR5639589.fastq.gz fmaleb_SRR5639589.fastq.gz
mv SRR5639584.fastq.gz malegerm_SRR5639584.fastq.gz
mv SRR5639585.fastq.gz malegerm_SRR5639585.fastq.gz
mv SRR5639586.fastq.gz malegerm_SRR5639586.fastq.gz
mv SRR5639581.fastq.gz fmalegerm_SRR5639581.fastq.gz
mv SRR5639582.fastq.gz fmalegerm_SRR5639582.fastq.gz
mv SRR5639583.fastq.gz fmalegerm_SRR5639583.fastq.gz

```

 Quality control using Trimmomatic, the preferred trimming tool for reads sequenced on the Illumina platform.  

```
echo `hostname`
#################################################################
# Trimming of Reads using Trimmomatic
#################################################################
module load Trimmomatic/0.39
java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/fmaleb_SRR5639588.fastq.gz \
        fmaleb_SRR5639588_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45
java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/fmaleb_SRR5639587.fastq.gz \
        fmaleb_SRR5639587_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45
java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/fmaleb_SRR5639589.fastq.gz \
        fmaleb_SRR5639589_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45
java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/maleb_SRR5639590.fastq.gz \
        maleb_SRR5639590_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45
java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/maleb_SRR5639591.fastq.gz \
        maleb_SRR5639591_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45

	java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/maleb_SRR5639592.fastq.gz \
        maleb_SRR5639592_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45

java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/fmalegerm_SRR5639581.fastq.gz \
        fmalegerm_SRR5639581_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45

java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/fmalegerm_SRR5639582.fastq.gz \
        fmalegerm_SRR5639582_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45

java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/fmalegerm_SRR5639583.fastq.gz \
        fmalegerm_SRR5639583_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45

java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/malegerm_SRR5639584.fastq.gz \
        malegerm_SRR5639584_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45

java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/malegerm_SRR5639585.fastq.gz \
        malegerm_SRR5639585_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45

java -jar $Trimmomatic SE \
        -threads 12 \
        ../raw_data/malegerm_SRR5639586.fastq.gz \
        malegerm_SRR5639586_trim.fastq.gz \
        ILLUMINACLIP:TruSeq3-SE.fa:2:30:10 \
        SLIDINGWINDOW:4:20 \
        MINLEN:45
```
Estimation of quality using FASTQC and MULTIQC. FASTQC performs multiple quality checks on the reads, such as sequence duplication level and per base sequence quality, and summarizes the findings in a graph. MULTIQC summarizes the findings of several FASTQC reports for different samples into one file. 

```

#################################################################
# FASTQC of trimmed reads
#################################################################
dir="after"
if [ ! -d "$dir" ]; then
        mkdir -p $dir
fi

module load fastqc/0.11.7
fastqc --outdir ./"$dir"/ ../quality_control/fmaleb_SRR5639587_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/fmaleb_SRR5639588_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/fmaleb_SRR5639589_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/maleb_SRR5639590_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/maleb_SRR5639591_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/maleb_SRR5639592_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/malegerm_SRR5639584_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/malegerm_SRR5639585_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/malegerm_SRR5639586_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/fmalegerm_SRR5639581_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/fmalegerm_SRR5639582_trim.fastq.gz -t 8
fastqc --outdir ./"$dir"/ ../quality_control/fmalegerm_SRR5639583_trim.fastq.gz -t 8

echo `hostname`

#################################################################
# MULTIQC of trimmed reads
#################################################################
module load MultiQC/1.8

multiqc --outdir trimmed_multiqc ./after/
```

![fastqc_sequence_counts_plot](https://user-images.githubusercontent.com/80122814/117383612-2482e500-aeaf-11eb-9101-c6999a766c75.png)
![fastqc_sequence_duplication_levels_plot (2)](https://user-images.githubusercontent.com/80122814/117383632-32d10100-aeaf-11eb-87e2-2f738d7a4e61.png)