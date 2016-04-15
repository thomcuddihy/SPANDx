# SPANDx
SPANDx - a genomics pipeline for comparative analysis of large haploid whole-genome re-sequencing datasets

*****SPANDx now works with SLURM, SGE and PBS (Torque) resource managers (v2.7+)*****

SPANDx can also be run directly without a resource handler (set SCHEDULER=NONE in scheduler.config), although this is not recommended.


<i>Why use SPANDx?</i>

SPANDx is your one-stop tool for identifying SNP and indel variants in haploid genomes using NGS data. SPANDx performs alignment of raw NGS reads against your chosen reference genome or pan-genome, followed by accurate variant calling and annotation, and locus presence/absence determination. SPANDx produces handy SNP and indel matrices for downstream phylogenetic analyses. Annotated, genome-wide SNPs and indels are identified and output in human-readable format. A presence/absence matrix is also generated to allow you to identify the core/accessory genome content across all your genomes.


USAGE: SPANDx.sh 
<parameters, required> 
-r <reference, without .fasta extension> 
[parameters, optional] 
-o [organism] 
-m [generate SNP matrix yes/no] 
-i [generate indel matrix yes/no] 
-a [include annotation yes/no] 
-v [Variant genome file. Name must match the SnpEff database] 
-s [Specify read prefix to run single strain] 
-t [Sequencing technology used Illumina/Illumina_old/454/PGM] 
-p [Pairing of reads PE/SE] 
-w [Window size in base pairs for BEDcoverage module]
-z [include tri- and tetra-allelic SNPs in the SNP matrix yes/no]

SPANDx, by default, expects reads to be paired-end, Illumina data in the following format: STRAIN_1_sequence.fastq.gz (first pair) and STRAIN_2_sequence.fastq.gz (second pair). 
Reads not in this format will be ignored.
If your data are not paired, you must set the -p parameter to SE to denote unpaired reads. By default -p is set to PE.

SPANDx requires a reference file in FASTA format. 
For compatibility with all steps in SPANDx, FASTA files should conform to the specifications listed here: http://www.ncbi.nlm.nih.gov/BLAST/blastcgihelp.shtml.
Note that the use of nucleotides other than A, C, G, or T is not supported by certain programs in SPANDx so should not be used in reference FASTA files. 
In addition, Picard, GATK and SAMtools handle spaces within contig names differently. Therefore, please do not use spaces or special characters (e.g. $/*) in contig names.

By default, all reads in SPANDx format (i.e. strain_1_sequence.fastq.gz) in the present working directory are processed. 
Sequence files are aligned against the reference using BWA. Alignments are subsequently filtered and converted using SAMtools and Picard Tools.
SNPs and indels are identified with GATK and coverage assessed with BEDtools. 

All variants identified in the single genome analysis are merged and re-verified across the entire dataset to minimise incorrect variant calls. This error-correction step is an attempt at establishing a "Best Practice" guideline for datasets where variant recalibration is not yet possible due to a lack of comprehensive quality-assessed variants (i.e. most haploid datasets!). Such datasets are available for certain eukaryotic species (e.g. human, mouse genomes), as detailed in GATK "Best Practice" guidelines.

Finally, SPANDx merges high-quality, re-verified SNP and indel variants into user-friendly .nex matrices, which can be used for phylogenetic resconstruction using various phylogenetics tools (e.g. PAUP, PHYLIP, RAxML).

SPANDx was written by: Dr Derek Sarovich and Dr Erin Price, Menzies School of Health Research, Darwin, NT, Australia.

Please send bug reports to mshr.bioinformatics@gmail.com or derek.sarovich@menzies.edu.au.

SPANDx citation: Sarovich DS & Price EP. 2014. SPANDx: a genomics pipeline for comparative analysis of large haploid whole genome re-sequencing datasets. <i>BMC Res Notes</i> 7:618.
