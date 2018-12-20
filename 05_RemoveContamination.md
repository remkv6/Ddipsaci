# NCBI found bacterial contamination in the D. dipsaci scaffolds, need to remove them from all output files.


```
#/work/GIF/remkv6/Baum/06_D_dipsaci/10_SubtractContamination

ln -s ../09_braker/braker/Ddipsaci/genome.fa
ln -s ../09_braker/braker/Ddipsaci/augustus.gff3
ln -s ../09_braker/braker/Ddipsaci/augustus.gene.fasta
ln -s ../09_braker/braker/Ddipsaci/augustus.cds.fasta
ln -s ../09_braker/braker/Ddipsaci/augustus.pep.fasta
ln -s ../02_repeatmodelerMasker/long_long2_v2_rascaf_medusa_curated.out.gff


#Scaffolds in question
################################################################################
Sequence name, length, span(s), apparent source
Scaffold_1013	366022	189827..199832,200124..236941,264374..304491	Acinetobacter sp.
Scaffold_1171	200485	14934..55163,74028..107916	Acinetobacter pittii
Scaffold_12	200519	47511..48615,48963..58603,150647..197150	Acinetobacter pittii,Acinetobacter sp.
Scaffold_1496	82216	12776..32889,53948..82216	Acinetobacter sp.
Scaffold_1545	87942	46848..67109	Acinetobacter pittii
Scaffold_1573	85494	20456..43916	Acinetobacter sp.
################################################################################

#Summarized Scaffolds
################################################################################
Scaffold_1013:189827-304491
Scaffold_1171:14934-107916
Scaffold_12:47511-197150
Scaffold_1496:12776-82216
Scaffold_1545:46848-67109
Scaffold_1573:20456-43916
###############################################################################


#Regions of the above scaffolds that may not be contaminants
###############################################################################
Scaffold_1013:1-189827
Scaffold_1013:236941-264374
Scaffold_1013:304491-366022
Scaffold_1171:107916-200485
Scaffold_12:1-47511
Scaffold_1545:1-46848
Scaffold_1545:67109-87942
Scaffold_1573:1-20456
Scaffold_1573:43916-85494
################################################################################
```

### Megablast the scaffolds above
```
#Copy the above list to a file for extraction with samtools faidx
vi ScafList.list
module load samtools
samtools faidx genome.fa
less ScafList.list |while read line; do samtools faidx ../genome.fa $line ; done >ScafList.fasta



sh runMegablast.sh ScafList.fasta

#runMegablast.sh
###############################################################################
#!/bin/bash

module load blast-plus/2.7.1-py2-vvbzyor
FASTA="$1"
blastn \
-task megablast \
-query ${FASTA} \
-db /work/GIF/GIF3/arnstrm/Baum/GenePrediction_Hg_20160115/05_databases/nt/nt \
-outfmt '6' \
-culling_limit 5 \
-num_threads 16 \
-evalue 1e-5 \
-out ${FASTA%.**}.vs.nt.cul5.1e25.megablast.out

###############################################################################

#All acinetobacter scaffolds
Scaffold_1013:1-189827  CP002080.1      90.880  82227   6329    1027    90167   171496  133030  214983  0.0     1.092e+05
Scaffold_1013:1-189827  CP002080.1      91.700  47119   3448    426     1       46766   46188   93196   0.0     64918
Scaffold_1013:1-189827  CP002080.1      91.713  18378   1404    97      171553  189827  216379  234740  0.0     25390
Scaffold_1013:1-189827  CP002080.1      90.281  13654   1086    222     69779   83268   115898  129474  0.0     17640

#acinetobacter scaffold
Scaffold_1013:236941-264374     CP014651.1      96.459  26208   730     186     1       26042   247315  273490  0.0     43074

#acinetobacter scaffold
Scaffold_1013:304491-366022     CP002177.1      96.484  55967   1397    535     6033    61532   2839746 2783884 0.0     91921
Scaffold_1013:304491-366022     CP002177.1      97.406  5706    137     11      1       5697    2845804 2840101 0.0     9707

#acinetobacter scaffold
Scaffold_1171:107916-200485     CP014651.1      95.865  77524   2334    791     12700   89483   3404093 3326702 0.0     1.246e+05
Scaffold_1171:107916-200485     CP014651.1      97.338  12812   214     119     1       12709   3417919 3405132 0.0     21653

#acinetobacter scaffold
Scaffold_12:1-47511     CP014651.1      96.792  47008   400     1036    1       46070   3596674 3643511 0.0     77431

#acinetobacter scaffold
Scaffold_1545:1-46848   CP014651.1      96.029  47420   1168    571     1       46848   969983  922707  0.0     76476

#acinetobacter scaffold
Scaffold_1545:67109-87942       CP014651.1      95.510  21069   609     318     1       20834   902325  881359  0.0     33355

Scaffold_1573:1-20456   CP014477.1      96.260  20562   643     122     1       20456   3818729 3839270 0.0     33595

#acinetobacter scaffold
Scaffold_1573:43916-85494       CP014651.1      95.294  42113   1313    588     1       41579   1060049 1018072 0.0     66170

```

### Remove scaffolds
```
#/work/GIF/remkv6/Baum/06_D_dipsaci/10_SubtractContamination

#create a list of just those scaffolds
vi Scaffs2remove.list

#create index and extract
cdbfasta genome.fa

#how many seqs does this leave from 1400
grep ">" long_long2_v2_rascaf_medusa_curated |sed 's/>//g' |grep -w -v -f Scaffs2remove.list |wc
   1394    1394   18981

#How many seq extracted?
grep ">" long_long2_v2_rascaf_medusa_curated |sed 's/>//g' |grep -w -v -f Scaffs2remove.list |cdbyank long_long2_v2_rascaf_medusa_curated.cidx >GenomeRevised.fa
grep -c ">" GenomeRevised.fa
1394
```

### Remove gff lines in augustus.gff3 and from repeats.gff
```
awk '$1!="Scaffold_1013" && $1!="Scaffold_1171" && $1!="Scaffold_12" && $1!="Scaffold_1496" && $1!="Scaffold_1545" && $1!="Scaffold_1573"' augustus.gff3 >augustusRevised.gff3

#How many to start with?
wc augustus.gff3
  425759  3591020 37181460 augustus.gff3

#How many are left?
wc augustusRevised.gff3
  424029  3575450 37013110 augustusRevised.gff3


awk '$1!="Scaffold_1013" && $1!="Scaffold_1171" && $1!="Scaffold_12" && $1!="Scaffold_1496" && $1!="Scaffold_1545" && $1!="Scaffold_1573"' long_long2_v2_rascaf_medusa_curated.out.gff>RepeatsRevised.gff

#How many to start with?
wc long_long2_v2_rascaf_medusa_curated.out.gff
  487440  5849250 48067498 long_long2_v2_rascaf_medusa_curated.out.gff

#How many are left?
less RepeatsRevised.gff |wc
 487427 5849094 48066186

```

### Remove Genes from gene.fasta cds.fasta and pep.fasta
```


awk '$1=="Scaffold_1013" || $1=="Scaffold_1171" || $1=="Scaffold_12" || $1=="Scaffold_1496" || $1=="Scaffold_1545" || $1=="Scaffold_1573" ' augustus.gff3 |awk '$3=="mRNA"' |awk '{print $9}' |sed 's/ID=//g' |sed 's/;/\t/g' |awk '{print $1}' >mRNAs2Remove.list
awk '$1=="Scaffold_1013" || $1=="Scaffold_1171" || $1=="Scaffold_12" || $1=="Scaffold_1496" || $1=="Scaffold_1545" || $1=="Scaffold_1573" ' augustus.gff3 |awk '$3=="gene"' |awk '{print $9}' |sed 's/ID=//g' |sed 's/;/\t/g' |awk '{print $1}' >Genes2remove.list

How many mRNA's are there on these scaffolds?
wc mRNAs2Remove.list
 415  415 3957 mRNAs2Remove.list

How many genes are there on these scaffolds?
wc Genes2remove.list
 415  415 3957 Genes2remove.list


## Remove from cds
 cdbfasta augustus.cds.fasta

#How many are there total
grep -c ">" augustus.cds.fasta
28479

grep ">" augustus.cds.fasta |sed 's/>//g' |grep -w -v -f mRNAs2Remove.list |cdbyank augustus.cds.fasta.cidx >augustus.cdsRevised.fasta

#How many are there after removal
grep -c ">" augustus.cdsRevised.fasta
28064


## Remove from peptides
cdbfasta augustus.pep.fasta

#how many at start?
grep -c ">" augustus.pep.fasta
28479

grep ">" augustus.pep.fasta |sed 's/>//g' |grep -w -v -f mRNAs2Remove.list |cdbyank augustus.pep.fasta.cidx >augustus.pepRevised.fasta

#how many after removal
grep -c ">" augustus.pepRevised.fasta
28064

## remove from genes
cdbfasta augustus.gene.fasta

How many to start with?
grep -c ">" augustus.gene.fasta
26843

 grep ">" augustus.gene.fasta |sed 's/>//g' |grep -w -v -f Genes2remove.list |cdbyank augustus.gene.fasta.cidx >augustus.geneRevised.fasta

#how many genes left?
grep -c ">" augustus.geneRevised.fasta
26428
```
