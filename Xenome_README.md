
* The Xenome source code is available at Gossamer bioinformatics suite [GitHub](https://github.com/data61/gossamer)

* Detailed usage of Xenome is avaliable at [GitHub](https://github.com/data61/gossamer/blob/master/docs/xenome.md)

```
xenome index -T 8 -P <index_prefix> -H <mouse_mm10.fa> -G <human_hg38.fa>
xenome classify -T 8 -P <index_prefix> --pairs --host-name mouse --graft-name human -i <in_1.fastq> -i <in_2.fastq>
```
```
docker run -v /mnt/biogalaxy:/tmp -w /tmp hsun9/xenome xenome classify -T 12 --pairs -P <Xenome_index> -i <R1_001.fastq> -i <R2_001.fastq> --graft-name human --host-name mouse --output-filename-prefix <output_file_prefix>
```


* The reads classified into human (graft) are mapped to human genome (hg38) using HISAT2. The reads classified into mouse (host) are mapped to mouse genome (mm10) using HISAT2.

The aligned SAM/BAM files of each human samples are performed gene expression analysis in TPM value by StringTie and merged the gene abundance files into one tab-delimited file using linux commands for downstream statistical analysis.

