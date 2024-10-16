# Altimeter processing baseline

## version 4.0

### Retracking
Some missions were retracked specifically for the CCI Sea State dataset, 
using WHALES retracker, whereas in some cases the data retracked by other 
agencies were used instead. 

| source                   | period             | retracker     | comment |
|--------------------------|--------------------|---------------|---------|
| ERS-1                    | 07/1991 to 03/2000 | REAPER (MLE3) |         |
| ERS-2                    | 04/1995 to 07/2011 | REAPER (MLE3) |         |
| Jason-1 Version E        | 01/2002 to 07/2013 | WHALES        |         |
| Jason-2 Version D        | 06/2008 to 10/2019 | WHALES        |         |
|                          |                    | WHALES        |         |
| Jason-3 Version D        | 09/2016 to 06/2019 | WHALES        |         |
| Jason-3 Version F        | 06/2019 to now     | WHALES        |         |
| Jason-3 Version T        | 02/2016 to 09/2016 | WHALES        |         |
| Topex Version F          | 08/1992 to 01/2006 |               |         |
| Envisat Version 3        | 03/2002 to 04/2012 | WHALES        |         |
| CryoSat-2  Version E     | 04/2010 to now     | WHALES        |         |
| SARAL Version T          | 02/2013 to now     | WHALES        |         |
| Sentinel-6 A Version F08 | 03/2020 to 12/2023 |               |         |
| Sentinel-6 A Version F09 | 12/2023 to now     |               |         |
| Sentinel-3 A Version 005 | 02/2016 to now     |               |         |
| Sentinel-3 B Version 005 | 04/2018 to now     |               |         |


### Compression to 1 Hz

#### Land detection
- full resolution (20/40 Hz) SWH and sigma0 values are flagged as land when 
  their distance to coast is **greater than 1000m**, based on the Goddard Space 
  Flight Center 1km resolution grid of distance to coast

#### Significant wave height

- SWH for uncompressed (20/40 Hz) measurements are discared if not in the 
  range: [-0.5, 30]

| Source                  | SWH                    | SWH quality                         |
|-------------------------|------------------------|-------------------------------------|
| Jason-1 Version E       | swh.07                 | swh_WHALES_fitting_error_20hz > 0.3 |
| Jason-2 Version D       | swh.07                 | swh_WHALES_fitting_error_20hz > 0.3 |
| Jason-3 Version D       | swh_WHALES_20hz        | swh_WHALES_fitting_error_20hz > 0.3 |
| Jason-3 Version F       | swh_WHALES_20hz        | swh_WHALES_qual_20hz                |
| Jason-3 Version T       | swh_WHALES_20hz        | swh_WHALES_fitting_error_20hz > 0.3 |
| Topex Version F         | swh_20hz_ku            | swh_used_20hz_ku                    |
| Envisat Version 3       | swh_WHALES_20hz        | swh_WHALES_qual_20hz                |
| ERS-1 REAPER            | swh_20hz               | swh_used_20hz == 0                  |
| ERS-2 REAPER            | swh_20hz               | swh_used_20hz == 0                  |
| CryoSat-2 Vesrion E     |                        |                                     |
| Saral Version T         |                        |                                     |
| Sentinel-6A Version F08 | swh_ocean              | swh_ocean_qual == 1                 |
| Sentinel-6A Version F09 | swh_ocean              | swh_ocean_qual ==1                  |
| Sentinel-3A Version 005 | swh_ocean_20_plrm_ku   | swh_ocean_qual_20_plrm_ku == 0      |
| Sentinel-3B Version 005 | swh_ocean_20_plrm_ku   | swh_ocean_qual_20_plrm_ku == 0      |




#### Sigma0
- sigma0 are **always taken from SGDR**, in C and Ku band (Ka for SARAL) - 
  using MLE3 Ku band sigma0 when available

| Source                   | Sigma0                | Sigma0 quality                           |
|--------------------------|-----------------------|------------------------------------------|
| Jason-1 Version E        | sig0_20hz_ku          | sig0_used_20hz_ku == 0                   |
|                          | sig0_20hz_c           | sig0_used_20hz_c == 0                    |
| Jason-2 Version D        | sig0_20hz_ku_mle3     | sig0_used_20hz_ku_mle3 == 0              |
|                          | sig0_20hz_c           | sig0_used_20hz_c == 0                    |
| Jason-3 Version D        | sig0_20hz_ku_mle3     | sig0_used_20hz_ku_mle3 == 0              |
|                          | sig0_20hz_c           | sig0_used_20hz_c == 0                    |
| Jason-3 Version F        | ku_sig0_ocean_mle3    | ku_sig0_ocean_mle3_compression_qual == 0 |
|                          | c_sig0_ocean          | c_sig0_ocean_compression_qual == 0       |
| Jason-3 Version T        |                       |                                          |
| Topex Version F          | sig0_20hz_ku_mle3     | sig0_used_20hz_ku == 0                   |
| Envisat Version 3        |                       |                                          |
| ERS-1 REAPER             | ocean_sig0_20hz       | ocean_sig0_used_20hz                     |
| ERS-2 REAPER             | ocean_sig0_20hz       | ocean_sig0_used_20hz                     |
| CryoSat-2  Version E     |                       |                                          |
| SARAL Version T          | sig0_40hz             | sig0_used_40hz == 0                      |
| Sentinel-6 A Version F08 | ku_sig0_ocean_mle3    | c_sig0_ocean_qual != 1                   |
|                          | c_sig0_ocean          | ku_sig0_ocean_mle3_qual != 1             |
| Sentinel-6 A Version F09 | ku_sig0_ocean_mle3    | c_sig0_ocean_qual != 1                   |
|                          | c_sig0_ocean          | ku_sig0_ocean_mle3_qual != 1             |
| Sentinel-3 A Version 005 | sig0_ocean_20_plrm_ku | sig0_ocean_qual_20_plrm_ku == 0          |
|                          | sig0_ocean_20_c       | sig0_ocean_qual_20_c == 0                |
| Sentinel-3 B Version 005 | sig0_ocean_20_plrm_ku |                                          |
|                          | sig0_ocean_20_c       |                                          |

```
S-band sigma0 were discared for Envisat as it was found they were systematically
flagged as bad in the SGDR product.
```


### L2P Processing

#### Ancillary atmosphere model output

**ERA5 model**

| Variable | Long name                       |
|----------|---------------------------------|
| tclw     | Total column cloud liquid water |
| t2m      | 2 metre temperature             |
| sst      | Sea surface temperature         |
| u10      | 10 metre U wind component       |
| v10      | 10 metre V wind component       |
| sp       | Surface pressure                |


#### Ancillary wave model output

**ERA5 model**

| Variable   | Long name                                           |
|------------|-----------------------------------------------------|
| swh        | Significant height of combined wind waves and swell |
| pp1d       | Peak wave period                                    |
| p1ps       | Mean wave period based on first moment of swell     |
| p140121    | Significant wave height of first swell partition    |
| p140122    | Mean wave direction of first swell partition        |
| mwp        | Mean wave period                                    |
| mwd        | Mean wave direction                                 |
| shww       | Significant height of wind waves                    |
| mdww       | Mean direction of wind waves                        |
| mpww       | Mean period of wind waves                           |


**WW3 model**

| Variable   | Long name                                  |
|------------|--------------------------------------------|
| uwnd       | eastward_wind                              |
| vwnd       | northward_wind                             |
| hs         | Significant height of wind and swell waves |
| t02        | Mean period T02                            |
| t0m1       | Mean period T0m1                           |
| 1/fp       | Wave peak frequency                        |
| dir        | Wave mean direction                        |
| skw        | skewness                                   | 
| qkk        | k-peakedness                               |

#### Ancillary sea ice concentration output

|                      | Variable | Long name                                                                                                              |
|----------------------|----------|------------------------------------------------------------------------------------------------------------------------|
| OSISAF-ICDR-v2p0     | ice_conc | Fully filtered concentration of sea ice using atmospheric correction of brightness temperatures and open water filters |
| OSISAF-AMSR-CDR-v3p0 | ice_conc | Fully filtered concentration of sea ice using atmospheric correction of brightness temperatures and open water filters |

