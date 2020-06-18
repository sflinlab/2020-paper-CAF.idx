### Figure 6
author: sufang
date: 6/18/2020
_______

#### Filtering steps to obtain the 9,356 patient data across 32 cancer subtypes through USC Xena-Browser
https://xenabrowser.net/
GDC Pan-Cancer (PANCAN)

##### program (19189->14522)
```
=TCGA
```
##### sample_type (14522->10986)
```
=Primary Tumor
```

##### Sample_ID (10986->10700)
```
A:-01A
```

##### OS/OS.time (10700->10230)
```
!=null
```
##### Gene Expression (e.g. A1BG) (10230->9356)
```
!=null
```


_______
#### Remove test samples in MET500
https://xenabrowser.net/
MET500 (expression centric)

##### test (868->811)
```
!=TRUE
```

#### Done and enjoy :accept: 




