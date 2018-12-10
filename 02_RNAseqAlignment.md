# Align RNA-seq for gene prediction


D. dipsaci reads
```
#/work/GIF/remkv6/Baum/06_D_dipsaci/04_AlignRNADDipsaci
module load hisat2
module load samtools
hisat2-build ../02_repeatmodelerMasker/long_long2_v2_rascaf_medusa_curated.masked DdipsaciMasked

sh runHISAT2.sh Ddi_Ontario_01_R01V1_RNA_S00107E_trim_R1.fq.gz Ddi_Ontario_01_R01V1_RNA_S00107E_trim_R2.fq.gz
#runHISAT2.sh
###############################################################################
#!/bin/bash

module load hisat2
module load samtools
DBDIR="/work/GIF/remkv6/Baum/06_D_dipsaci/04_AlignRNADDipsaci"
GENOME="DdipsaciMasked"

p=12
R1_FQ="$1"
R2_FQ="$2"

OUTPUT=$(basename ${R1_FQ} |cut -f 1 -d "_");

hisat2 \
  -p ${p} \
  --score-min L,0,-0.2 \
  -x ${DBDIR}/${GENOME} \
  -1 ${R1_FQ} \
  -2 ${R2_FQ}  \
  -S  ${OUTPUT}.sam &> ${OUTPUT}.log
samtools view --threads 12 -b -o ${OUTPUT}.bam ${OUTPUT}.sam
samtools sort -m 2G -o ${OUTPUT}_sorted.bam -T ${OUTPUT}_temp --threads 12 ${OUTPUT}.bam

###############################################################################

samtools flagstat -@ 16 Ddi_sorted.bam
###############################################################################

34578370 + 0 in total (QC-passed reads + QC-failed reads)
3458400 + 0 secondary
0 + 0 supplementary
0 + 0 duplicates
27788066 + 0 mapped (80.36% : N/A)
31119970 + 0 paired in sequencing
15559985 + 0 read1
15559985 + 0 read2
22284086 + 0 properly paired (71.61% : N/A)
22810914 + 0 with itself and mate mapped
1518752 + 0 singletons (4.88% : N/A)
248904 + 0 with mate mapped to a different chr
222035 + 0 with mate mapped to a different chr (mapQ>=5)
##############################################################################
80% of the reads mapping seems reasonable with all of the repeats masked.  

```


D. destructor reads
```
/work/GIF/remkv6/Baum/06_D_dipsaci/01_D_destructorReads


sh runHISAT2.sh SRR7943144_1_val_1.fq.gz SRR7943144_2_val_2.fq.gz


#ran many different parameters, but did not ever get better than 0.01% of reads aligning paired
samtools flagstat -@ 16 SRR7943144_sorted.bam
###############################################################################
56811777 + 0 in total (QC-passed reads + QC-failed reads)
1961 + 0 secondary
0 + 0 supplementary
0 + 0 duplicates
3836 + 0 mapped (0.01% : N/A)
56809816 + 0 paired in sequencing
28404908 + 0 read1
28404908 + 0 read2
58 + 0 properly paired (0.00% : N/A)
80 + 0 with itself and mate mapped
1795 + 0 singletons (0.00% : N/A)
0 + 0 with mate mapped to a different chr
0 + 0 with mate mapped to a different chr (mapQ>=5)
###############################################################################

```
