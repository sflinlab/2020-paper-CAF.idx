The orginial Xenome source code is available from Gossamer bioinformatics suite 
https://github.com/data61/gossamer

Detailed usage of Xenome is avaliable at
The https://github.com/data61/gossamer/blob/master/docs/xenome.md

'''
xenome index -T 8 -P <index_prefix> -H <mouse_mm10.fa> -G <human_hg38.fa>
xenome classify -T 8 -P <index_prefix> --pairs --host-name mouse --graft-name human -i <in_1.fastq> -i <in_2.fastq>
'''
'''
docker run -v /mnt/biogalaxy:/tmp -w /tmp hsun9/xenome xenome classify -T 12 --pairs -P 2017_HuMouse_EBV/Mouse_Genome/PDX_index -i 2019_IMB_PDX/Raw/YCK-3_S2_R1_001.fastq -i 2019_IMB_PDX/Raw/YCK-3_S2_R2_001.fastq --graft-name human --host-name mouse --output-filename-prefix 2019_IMB_PDX/analysis/YCK-3/YCK-3
'''

The reads classified into human (graft) are mapped to human genome (hg38) using HISAT2.
The reads classified into mouse (host) are mapped to mouse genome (mm10) using HISAT2.

The aligned SAM/BAM files of each human samples are performed gene expression analysis in TPM value by StringTie and merged the gene abundance files into one tab-delimited file using linux commands for downstream statistical analysis.

