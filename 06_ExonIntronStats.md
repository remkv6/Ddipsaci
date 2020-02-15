# How many introns exons/gene, and what was their mean length
```
#/work/GIF/remkv6/Baum/06_D_dipsaci/10_SubtractContamination


#convert the cds in col 3 to exon.  
less IntronsaugustusRevised.gff3 |grep -v "#" |sed 's/CDS/exon/g' >ConvertCDS2EXON.gff
module load genometools/1.5.9-py2-awelg6n

gt gff3 -addintrons -tidy ConvertCDS2EXON.gff >IntronsaugustusRevised.gff3


#average number of exons per gene
less IntronsaugustusRevised.gff3 |awk '$3=="exon"' |cut -f 9- |sort |uniq -c |awk '{print $1}' |summary.sh
less IntronsaugustusRevised.sh
Total:  209,748
Count:  28,064
Mean:   7
Median: 5
Min:    1
Max:    114

#28,064 genes, with mean of 7 exons per gene

#what is the average exon length
less IntronsaugustusRevised.gff3 |awk '$3=="exon"' |awk '{if ($4>$5) {print $4-$5} else {print $5-$4}}'|summary.sh
Total:  25,268,852
Count:  209,748
Mean:   120
Median: 98
Min:    2
Max:    9,194

The average exon size is 120 bp


#average number of introns per gene, that had more than one exon
less IntronsaugustusRevised.gff3 |awk '$3=="intron"' |cut -f 9- |sort |uniq -c |awk '{print $1}' |summary.sh
Total:  181,673
Count:  26,135
Mean:   6
Median: 5
Min:    1
Max:    113


#26,135 genes with an intron. In genes that had an intron, the average number of introns was 6

#what is the average intron length
less IntronsaugustusRevised.gff3 |awk '$3=="intron"' |awk '{if ($4>$5) {print $4-$5} else {print $5-$4}}'|summary.sh
Total:  93,630,180
Count:  181,673
Mean:   515
Median: 56
Min:    36
Max:    27,260

The average intron length is 515 bp

```
