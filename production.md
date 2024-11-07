# Production status

## Altimetry

This section summarizes the number of files produced at each processing step of 
the altimetry processing workflow. In case of discrepancies between subsequent 
processing steps, an explanation is provided.


```{table} Number of files produced at each step in the processing workflow.

| Mission         | SGDR files | Retracked files     | 1Hz files           | L2P files           |
|-----------------|------------|---------------------|---------------------|---------------------|
| JASON-1         | 102 158    | 92 099 [^footnote1] | 92 099              | 92 018 [^footnote3] |
| JASON-2         | 98 619     | 98 619              | 98 619              | no data yet         |
| JASON-3D        | 29 771     | 29 631 [^footnote2] | 29 606 [^footnote3] | no data yet         |
| JASON-3F        | 44 222     | 44 362 [^footnote2] | 42 196 [^footnote3] | no data yet         | 
| SARAL           | 117 339    | 87 620 [^footnote1] | no data yet         | no data yet         |
| CRYOSAT-2       | 778 522    | 776 245 [^footnote1]| 175 710 [^footnote3]| no data yet         |
| ERS-1           |            | 24 461              | 24 448 [^footnote3] | 17 340 [^footnote3] |
| ERS-2           |            | 43 058              | 43 058              | 43 057              |
| ENVISAT         |            | 99 771              | 99 771              | 99 771              |
| TOPEX           | 117 669    | 117 669             | 110 178 [^footnote4]| no data yet         |
| SENTINEL-3A     |            | 71 237              | 71 237              | 71 237              |
| SENTINEL-3B     |            | 50 318              | 50 318              | 50 318              |
| SENTINEL-6A-F08 |            | 30 163              | 30 163              | no data yet         |
| SENTINEL-6A-F09 |            | 32 823              | 32 823              | no data yet         |
```

[^footnote1]: Part of the retracking wasn't done, or some files generate errors. To be fixed.
[^footnote2]: Some retracked files are missing in version D, and have been replaced by retracked files in version F to fill the gaps. There are fewer retracked files than SGDR files in version D, and more retracked files than SGDR files in version F.
[^footnote3]: Some files need to be reprocessed, they're just missing. To be fixed.
[^footnote4]: Somes files generate errors. To be fixed.