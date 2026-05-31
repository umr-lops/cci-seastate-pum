# Changes

This sections summarizes the main changes with respect to the previous releases.

## version 5 [2026]

The version 5 extends the version 4 by 2 more years, up to December 2025. 

The GFO, CFOSAT and SWOT nadir altimeters have been added to the existing series. 
Waveforms from Jason-1, Jason-2, Jason-3, Sentinel-6 A, ERS-1, ERS-2, ENVISAT, 
Cryosat-2 and SWOT Nadir have been retracked using a common version of WHALES 
for improved consistency across all altimeter missions. A new editing (in 
particular the test on SWH RMS) and inter-calibration of all missions was 
performed. The latest release of CCI and OSI SAF sea ice concentrations was used 
for the editing of the sea ice contaminated observations.

The version 5 includes also a new preliminary "L2P Wind" dataset providing bias 
corrected $\sigma^0$ and derived wind speed for a limited set of altimeters (dual 
C and Ku band altimeters): Jason-1, Jason-2, Jason-3, Topex-Poseidon, Sentinel-3 A, 
Sentinel-3 B, Sentinel-6 A.

Last, the version 5 includes a new SAR derived dataset, containing full wave spectra 
and partitions from Sentinel-1 A and Sentinel-1 B.


## version 4 [2024/2025]

The version 4 extends the version 3 dataset up to December 2023 and fixes some 
minor issues or add some additional ancillary variables. 

Version 4 extends the altimeter data series of version 3 with 3 more years 
up to 31-Dec-2023 for the ongoing (Jason-3, SARAL, Cryosat-2) and newest 
(Sentinel-3 A, Sentinel-3 B, Sentinel-6 A) but also backward by adding more 
missions (ERS-1, ERS-2, Topex-Poseidon) back to 1992.

Version 4 also adds SAR altimetry data from Sentinel-1 A and Sentinel-1 B 
Image mode (IW and EW).


### L2P products
- [fixed and updated bathymetry (GEBCO 2024)](changes/v4/bathymetry)
- [changed calculation of land mask, based on distance to coast](changes/v4/landmask)
- [update version of jason-3 retracked files](changes/v4/jason3_versions)

## Version 3 [2021/2022]

The version 3 extends the version 2 dataset up to December 2020 and fixes some 
minor issues or add some additional ancillary variables. Version 3 also adds
SAR altimetry data from Sentinel-3, and microseism data.

## Version 2

The 2nd version (version 2) of CCI Sea State products provided a complete 
revised processing and methodology compared to the version 1 which was mainly 
inherited from the GlobWave project. It extended and improved the altimeter 
significant wave height (SWH) products which were a post-processing over 
existing L2 altimeter agency products with a complete new retracking of the 
considered missions for this version 2, and fully revised filtering, corrections
and uncertainty evaluations. The retracking of altimeter waveforms was 
performed with two different retrackers: WHALES by TUM for LRM altimeters,  
and LR-RMC by CLS/CNES for SAR altimeters. Because of the novelty of this 
approach and the required processing effort, this was only applied to a limited 
set of recent altimeters spanning from 2002 to 2018.

## Version 1

The version 1 was mainly inherited from the GlobWave project.


