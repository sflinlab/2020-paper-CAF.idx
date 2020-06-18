### Figure 4-old version
author: sufang
date: 6/18/2020
_______

#### Filtering steps to obtain 3,338 single cells through Xena-Browser
https://xenabrowser.net/
Head and Neck Cancer (Puram 2017)

##### Cell type (5902->4874)
```
B: "tumor cell" OR "T cell" OR "Fibroblast"
```
##### remove Patient (4874->3338)
```
C: !="0" AND !="6" AND !="20" AND !="26"
```

![](https://i.imgur.com/952LomZ.jpg)

