# Rna-seq from other species is aligning poorly, how do CDS sequences align?


### D. destructor
```
#/work/GIF/remkv6/Baum/06_D_dipsaci/05_DestructorTranscripts

#build database from masked genome
gmap_build -D /work/GIF/remkv6/Baum/06_D_dipsaci/05_DestructorTranscripts -d Ddipsaci ../02_repeatmodelerMasker/long_long2_v2_rascaf_medusa_curated.masked

sh runGmap.sh Ddipsaci /work/GIF/remkv6/Baum/06_D_dipsaci/05_DestructorTranscripts/ long_long2_v2_rascaf_medusa_curated.masked ditylenchus_destructor.PRJNA312427.WBPS11.CDS_transcripts.fa

#runGmap.sh
###############################################################################
#!/bin/bash

#Makes a database and searches your sequences.
#sh runGmap.sh <database name> <folder of database file ending with a "/"> <Fasta file> <query file>

#examples
#sh run_gmap.sh red_abalone_02Jun2017_5fUJu /work/GIF/remkv6/Serb/03_DavideGMAP/ red_abalone_02Jun2017_5fUJu.fasta DavideQuerydna.fasta
#sh run_gmap.sh  m.yessoensisGenome /work/GIF/remkv6/Serb/03_DavideGMAP DavideQuerydna.fasta
#sh run_gmap.sh Crassostreagigasgenome /work/GIF/remkv6/Serb/03_DavideGMAP Crassostreagigasgenome.fa DavideQuerydna.fasta


module load gmap-gsnap/2018-03-25-qa3kh3t
dbname=$1
dbloc=$2
dbfasta=$3
query=$4
gmap_build -d $dbname  -D $dbloc $dbfasta
gmap -D $dbloc -d $dbname -B 5 -t 16  --input-buffer-size=1000000 --output-buffer-size=1000000 -f gff3_gene  $query >${dbname%.*}.${query%.*}.gff3
###############################################################################


#how did they map?
grep -c ">" ditylenchus_destructor.PRJNA312427.WBPS11.CDS_transcripts.fa
13938
awk '$3=="gene"' Ddipsaci.ditylenchus_destructor.PRJNA312427.WBPS11.CDS_transcripts.gff3 |wc
   1593   14337  125196

~like 12%.  very poorly
```

Bursephelenchus xylophilus
```
#/work/GIF/remkv6/Baum/06_D_dipsaci/06_xylophilusTranscripts

sh runGmap.sh Ddipsaci /work/GIF/remkv6/Baum/06_D_dipsaci/05_DestructorTranscripts/ ../02_repeatmodelerMasker/long_long2_v2_rascaf_medusa_curated.masked bursaphelenchus_xylophilus.PRJEA64437.WBPS11.CDS_transcripts.fa

grep -c ">" bursaphelenchus_xylophilus.PRJEA64437.WBPS11.CDS_transcripts.fa
17704


awk '$3=="gene"' Ddipsaci.bursaphelenchus_xylophilus.PRJEA64437.WBPS11.CDS_transcripts.gff3 |wc
    773    6957   68401

```

Globodera rostochiensis
```
sh ../05_DestructorTranscripts/runGmap.sh Ddipsaci /work/GIF/remkv6/Baum/06_D_dipsaci/05_DestructorTranscripts/ ../02_repeatmodelerMasker/long_long2_v2_rascaf_medusa_curated.masked globodera_rostochiensis.PRJEB13504.WBPS11.CDS_transcripts.fa.gz

grep -c ">" globodera_rostochiensis.PRJEB13504.WBPS11.CDS_transcripts.fa.gz
11589
awk '$3=="gene"' Ddipsaci.globodera_rostochiensis.PRJEB13504.WBPS11.CDS_transcripts.fa.gff3 |wc
      0       0       0

WOW, none of them mapped... weird
```

Meloidogyne hapla
```
/work/GIF/remkv6/Baum/06_D_dipsaci/08_MhaplaTranscripts

sh ../05_DestructorTranscripts/runGmap.sh Ddipsaci /work/GIF/remkv6/Baum/06_D_dipsaci/05_DestructorTranscripts/ ../02_repeatmodelerMasker/long_long2_v2_rascaf_medusa_curated.masked meloidogyne_hapla.PRJNA29083.WBPS11.CDS_transcripts.fa

grep -c ">" meloidogyne_hapla.PRJNA29083.WBPS11.CDS_transcripts.fa
14420

awk '$3=="gene"'  Ddipsaci.meloidogyne_hapla.PRJNA29083.WBPS11.CDS_transcripts.gff3 |wc
    533    4797   60846


```
