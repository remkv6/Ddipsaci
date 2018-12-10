# Running braker on D. dipsaci rnaseq and D. destructor proteins.
```
#/work/GIF/remkv6/Baum/06_D_dipsaci/09_braker
ln -s ../04_AlignRNADDipsaci/Ddi_sorted.bam
ln -s ../02_repeatmodelerMasker/long_long2_v2_rascaf_medusa_curated.masked
module load GIF/braker/2.1.0
braker.pl --cores 12 --gff3 --species=Ddipsaci --genome=long_long2_v2_rascaf_medusa_curated.masked --bam=Ddi_sorted.bam --prot_seq=ditylenchus_destructor.PRJNA312427.WBPS11.protein.fa --prg=exonerate

```
### Output stats
```
#/work/GIF/remkv6/Baum/06_D_dipsaci/09_braker/braker/Ddipsaci

#Convert gtf to gff3
module load genometools
gt gtf_to_gff3 -force -tidy -o augustus.gff3 augustus.hints.gtf

#had to create a dummy entry in order for the below script to get all of the mRNA's.
less augustus.gff3 |grep -v "#" |awk '$3!="exon"'|cat - <(printf "Scaffold_942\tDummy\tmRNA\t12200\t12300\t-\t1\tParent=test\n")  >augustusMod.gff3
#Create the fasta sequences
~/common_scripts/gff2fasta.pl genome.fa augustusMod.gff3 augustus

#are all gene represented in the fasta sequences?
awk '$3=="gene"' augustus.gff3 |wc
  26843  241587 1813656
grep -c ">" augustus.gene.fasta
26843

#are all transcripts represented in the fasta sequences?
awk '$3=="mRNA"' augustus.gff3 |wc
  28479  256311 3097644
grep -c ">" augustus.cds.fasta
  28479
grep -c ">" augustus.pep.fasta
  28479
grep -c ">" augustus.cdna.fasta
  28479

```
