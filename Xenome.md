### Xenome codes to generate Figure S1b
author: daniel
date: 6/16/2020
_______

For the original Xenome source code, please refer to Gossamer bioinformatics suite https://github.com/data61/gossamer
Detailed usage of Xenome is available at https://github.com/data61/gossamer/blob/master/docs/xenome.md

______
```
xenome index -T 8 -P  -H  -G 
xenome classify -T 8 -P  --pairs --host-name mouse --graft-name human -i  -i 
```

* For reads classifed as human (=graft) were aligned to the human genome (hg38) by using HISAT2.
* For reads classified as mouse (=host) were aligned to the mouse genome (mm10) by using HISAT2.

To generate the exprsssion matrix:
* Aligned SAM/BAM files of each samples were quantitated by using StringTie (output as TPM)
```
stringtie <Sample_1_mouse_hisat2.sorted.bam> -G <mouse.gencode.vM21.annotation.gtf> -p 24 --rf -A <Sample_1_mouse_gene_abund.2019.M21.out> -o <Sample_1_mouse_out.2019.M21.gtf> -e -B
```
* Results of all samples were merged into one tab-delimited file using linux commands for downstream analyses.
```
paste <sample 1 file><sample 2 file> ....> merge_human_gene_abund.sorted.out (big table)
cut -f2,9,18 .... merge_human_gene_abund.sorted.out > merge_human_gene_abund.sorted.TPM.out (TPM column of each sample)
```


