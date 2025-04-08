# Production status

## Altimetry

### Processing summmary

This section summarizes the number of files produced at each processing step of 
the altimetry processing workflow. In case of discrepancies between subsequent 
processing steps, an explanation is provided. Data are considered until 
31/12/2023.


```{table} Number of files produced at each step in the processing workflow.

| Mission         | SGDR files | Retracked files     | 1Hz files           | L2P files            | Index files |
|-----------------|------------|---------------------|---------------------|----------------------|-------------|
| JASON-1 (version E)   | 102 158    | 102 158             | 102 154 [^footnote4] | 102 154 [^footnote4]  |  |
| JASON-2 (version D)   | 98 619     | 98 619              | 98 619              | 98 619               | 98 619      |
| JASON-3 (version D)   | 26 947     | 26 947 [^footnote5] | 26 947              | 26 947               | 30 114      |
| JASON-3 (version F)   | 45 452     | 45 452              | 45 452              | 45 450               | 42 785      |
| SARAL (version F)     | 112 474    | 112 467 [^footnote3]| 112 467             | 112 439              | 0           |
| CRYOSAT-2 (version E) | 758 841    | 758 840 [^footnote1]| 615 255 [^footnote2]| 3 402                | 0           |
| ERS-1                 | 24 461     |                     | 24 461              |  | 24 290      |
| ERS-2                 | 43 058     |                     | 43 058              |                | 43 042      |
| ENVISAT               |            | 99 771              | 99 771              | 99 771               | 99 768      |
| TOPEX-A (version F)   | 52 304     |                     | 52 304              | 52 304         |    |
| TOPEX-B (version F)   | 57 825     |                     | 57 825              | 57 825         |    |
| SENTINEL-3A           | 79 738     |                     | 79 737 [^footnote6] | 79 737 [^footnote6]  | 79 737      |
| SENTINEL-3B           | 58 827     |                     | 58 820 [^footnote7] | 58 820 [^footnote7]  | 58 820      |
| SENTINEL-6A-F08       | 28 619     |                     | 28 351 [^footnote8] | 28 351 [^footnote8]  | 28 351      |
```

### Processing issues

Some issues noted during data verification:

- some ERS-1 / ERS-2 (REAPER) files contain anomalous measurement times (out of 
  the orbit time frame) while realistic. They were found associated with 
  zero-value latitude and longitude (but not always). TO BE FIXED.
- ERS-1 / ERS-2 (REAPER) : some zero-value SWH should be flagged out 
  (probably fill value)
- duplicated packets found in ERS-1 and ERS-2 data : duplicated sequences to 
  be removed from files, backward discontinuities : to be detected on 
  archive and fixed in output L2P
- duplicated files (with different production or coverage time) have been found 
  for different missions : to be cleaned
- issue with model interpolation for orbits spanning over two different days 
  or months : TO BE FIXED. (regenerate ancillary fields) 
- failed outlier test due to a python bug : S6 and Topex reprocessed, to be 
  done for other missions 
- fill value for ERS-1 / ERS-2 L2P lat/lon is 2147483647. instead of 1e20
- Data processed with WHALES at TUM (Jason-1, Jason-2, part of Jason-3) were 
  edited wrt a coarse land mask, leading to less data close to coastal areas.
  They should be reprocessed in CCI Sea State version 5.
- Some TUM processed data (Jason-3D) containing zero-value lat and/or lon for 
  some measurements. They have to be edited from WHALES files. **FIXED**.
- Cryosat-2 is split in numerous small files, which may create issues for 
  EMD denoising : investigate full orbit stitching (if no discontinuities between 
  files) : TO BE CHECKED 


[^footnote1]: Part of the retracking wasn't done, or some files generate errors. To be fixed.
[^footnote2]: Some files need to be reprocessed, they're just missing. To be fixed.
[^footnote3]: 6 SARAL SGDR files corrupted at provider; 1 SGDR generates dummy times when retracked (to be investigated).
[^footnote4]: 4 Jason-1 SGDR containing only one second of data were removed.
[^footnote5]: 3 Jason-3 version D WHALES products (processed by TUM) were dropped as they had invalid (zero) lat and/or lon. 
[^footnote6]: 1 Sentinel-3 A SGDR file is a duplicate (with a different production time)
[^footnote7]: 7 Sentinel-3 B SGDR files are duplicates (with a different production time) 
[^footnote8]: 268 SGDR duplicates removed