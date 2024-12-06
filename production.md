# Production status

## Altimetry

This section summarizes the number of files produced at each processing step of 
the altimetry processing workflow. In case of discrepancies between subsequent 
processing steps, an explanation is provided. Datas are considered until 31/12/2023.


```{table} Number of files produced at each step in the processing workflow.

| Mission         | SGDR files | Retracked files     | 1Hz files           | L2P files            | Index files |
|-----------------|------------|---------------------|---------------------|----------------------|-------------|
| JASON-1         | 102 158    | 102 158             | 92 099 [^footnote2] | 92 099 [^footnote2]  | 184 088     |
| JASON-2         | 98 619     | 98 619              | 98 619              | 98 619               | 98 619      |
| JASON-3D        | 26 947     | 26 947              | 26 947              | 26 947               | 30 114      |
| JASON-3F        | 45 452     | 45 452              | 45 452              | 45 450               | 42 785      |
| SARAL           | 112 474    | 112 467 [^footnote3]| 112 467             | 112 439              | 0           |
| CRYOSAT-2       | 758 841    | 758 840 [^footnote1]| 615 255 [^footnote2]| 3 402                | 0           |
| ERS-1           |            | 24 461              | 24 448 [^footnote2] | 24 290 [^footnote2]  | 24 290      |
| ERS-2           |            | 43 058              | 43 058              | 43 057               | 43 042      |
| ENVISAT         |            | 99 771              | 99 771              | 99 771               | 99 768      |
| TOPEX           | 117 669    | 117 669             | 110 129 [^footnote2]| 110 129 [^footnote2] | 110 129     |
| SENTINEL-3A     |            | 71 237              | 71 237              | 71 237               | 71 237      |
| SENTINEL-3B     |            | 50 318              | 50 318              | 50 318               | 50 318      |
| SENTINEL-6A-F08 |            | 28 619              | 28 349              | 28 349               | 28 349      |
```

[^footnote1]: Part of the retracking wasn't done, or some files generate errors. To be fixed.
[^footnote2]: Some files need to be reprocessed, they're just missing. To be fixed.
[^footnote3]: 6 SGDR files corrupted at provider; 1 SGDR generates dummy times when retracked (to be investigated).