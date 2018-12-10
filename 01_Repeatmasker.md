#  Repeat stats in the sent genome did not look complete, so repeating RepeatMasker with PY's consensi.classified file

```
#/work/GIF/remkv6/Baum/06_D_dipsaci/02_repeatmodelerMasker

module load repeatmasker/4.0.7-openmpi3-vruhfro
RepeatMasker -pa 16 -gff -nolow -lib consensii.classified long_long2_v2_rascaf_medusa_curated

################################################################################
==================================================
file name: long_long2_v2_rascaf_medusa_curated
sequences:          1400
total length:  228256690 bp  (228255290 bp excl N/X-runs)
GC level:         37.51 %
bases masked:  102682796 bp ( 44.99 %)
==================================================
               number of      length   percentage
               elements*    occupied  of sequence
--------------------------------------------------
SINEs:               73        12737 bp    0.01 %
      ALUs            0            0 bp    0.00 %
      MIRs            0            0 bp    0.00 %

LINEs:             2327       586862 bp    0.26 %
      LINE1        1522       130296 bp    0.06 %
      LINE2           0            0 bp    0.00 %
      L3/CR1        310       360729 bp    0.16 %

LTR elements:      9343      2792408 bp    1.22 %
      ERVL            0            0 bp    0.00 %
      ERVL-MaLRs      0            0 bp    0.00 %
      ERV_classI    373        40547 bp    0.02 %
      ERV_classII   791       168881 bp    0.07 %

DNA elements:     68942     15883185 bp    6.96 %
     hAT-Charlie    719       135459 bp    0.06 %
     TcMar-Tigger   683       104300 bp    0.05 %

Unclassified:    368180     81676494 bp   35.78 %

Total interspersed repeats:100951686 bp   44.23 %


Small RNA:          141        57391 bp    0.03 %

Satellites:        2288       616006 bp    0.27 %
Simple repeats:    6647      1466920 bp    0.64 %
Low complexity:       0            0 bp    0.00 %
==================================================

* most repeats fragmented by insertions or deletions
  have been counted as one element


The query species was assumed to be homo
RepeatMasker Combined Database: Dfam_Consensus-20170127

run with rmblastn version 2.6.0+
The query was compared to classified sequences in "consensii.classified"
###############################################################################


```


How much was masked?
```
~/common_scripts/new_Assemblathon.pl long_long2_v2_rascaf_medusa_curated.masked

###############################################################################
Number of scaffolds       1400
Total size of scaffolds  228256690
   Longest scaffold    3685767
  Shortest scaffold       1050
Number of scaffolds > 1K nt       1400 100.0%
Number of scaffolds > 10K nt       1365  97.5%
Number of scaffolds > 100K nt        670  47.9%
Number of scaffolds > 1M nt         22   1.6%
Number of scaffolds > 10M nt          0   0.0%
 Mean scaffold size     163040
Median scaffold size      93453
N50 scaffold length     287009
 L50 scaffold count        202
        scaffold %A      17.36
        scaffold %C      10.15
        scaffold %G      10.16
        scaffold %T      17.34
        scaffold %N      44.99
###############################################################################


```
