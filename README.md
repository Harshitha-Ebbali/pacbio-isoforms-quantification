# pacbio-isoforms-quantification
A streamlined pipeline for isoform-level quantification of PacBio Iso-Seq data🧬.

This repository walks you through a full Iso-Seq analysis pipeline using PacBio SMRT tools and Mandalorion, from raw subreads to isoform quantification. The main steps include:

1. Generating high-quality CCS reads

2. Demultiplexing barcoded samples with Lima

3. Refining reads to obtain full-length non-concatemer (FLNC) sequences

4. Converting refined BAM to FASTQ format

5. Running Mandalorion for isoform detection and quantification


**Step-by-Step Guide**

**1. Generate CCS Reads**

ccs m64137.subreads.bam isoseq-Hifi.bam --min-rq 0.9 -j 10

**2. Demultiplex with Lima**

lima isoseq-Hifi.bam barcdes.fa isoseq_ccs_demux.bam --isoseq --peek-guess --log-level INFO --log-file log.txt --split --split-named -j 10

**3. Refine Reads**

isoseq3 refine isoseq_ccs_demux.bam barcdes.fa isoseq_ccs_demux_flnc.bam -j 10'

**4. Convert BAM to FASTQ**

samtools fastq isoseq_ccs_demux_flnc.bam > isoseq_ccs_demux_flnc.fastq

**5. Mandalorian pipeline**

**Installation**

git clone https://github.com/christopher-vollmers/Mandalorion.git

cd Mandalorion

chmod +x setup.sh

sh setup.sh

python3 Mando.py -p working_dir -g reference.gtf -G reference.fasta -f Consensus_reads.fofn

.fofn file structure is simply a text file with one line per input fasta/fastq file. You can mix and match fasta/fastq files and gzipped and unzipped files.

An output file Isoforms.filtered.clean.quant: Number of reads from each supplied fasta file associated with each isoform, can be used for the differential expression analysis using DESeq or EdgeR.
