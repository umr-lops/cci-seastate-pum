# Altimeter processing baseline

## version 4.0

### Retracking
Some missions were retracked specifically for the CCI Sea State dataset, 
using WHALES retracker, whereas in some cases the data retracked by other 
agencies were used instead. 

| source                   | period | retracker     | comment |
|--------------------------|--------|---------------|---------|
| ERS-1                    |        | REAPER (MLE3) |         |
| ERS-2                    |        | REAPER (MLE3) |         |
| Jason-1 Version E        |        | WHALES        |         |
| Jason-2 Version D        |        | WHALES        |         |
|                          |        | WHALES        |         |
| Jason-3 Version D        |        | WHALES        |         |
| Jason-3 Version F        |        | WHALES        |         |
| Jason-3 Version T        |        | WHALES        |         |
| Topex Version F          |        |               |         |
| Envisat Version 3        |        | WHALES        |         |
| CryoSat-2  Version E     |        | WHALES        |         |
| SARAL Version T          |        | WHALES        |         |
| Sentinel-6 A Version F08 |        |               |         |
| Sentinel-6 A Version F09 |        |               |         |
| Sentinel-3 A Version 005 |        |               |         |
| Sentinel-3 B Version 005 |        |               |         |


### Compression to 1 Hz

#### Land detection
- full resolution (20/40 Hz) SWH and sigma0 values are flagged as land when 
  their distance to coast is **greater than 1000m**, based on the Goddard Space 
  Flight Center 1km resolution grid of distance to coast

#### Significant wave height

- SWH for uncompressed (20/40 Hz) measurements are discared if not in the 
  range: [-0.5, 30]


#### Sigma0
- sigma0 are **always taken from SGDR**, in C and Ku band (Ka for SARAL) - 
  using MLE3 Ku band sigma0 when available

| source                   | sigma0               | sigma0 quality                           |
|--------------------------|----------------------|------------------------------------------|
| Jason-1 Version E        | sig0_20hz_ku         | sig0_used_20hz_ku == 0                   |
|                          | sig0_20hz_c          | sig0_used_20hz_c == 0                    |
| Jason-2 Version D        | sig0_20hz_ku_mle3    | sig0_used_20hz_ku_mle3 == 0              |
|                          | sig0_20hz_c          | sig0_used_20hz_c == 0                    |
| Jason-3 Version D        | sig0_20hz_ku_mle3    | sig0_used_20hz_ku_mle3 == 0              |
|                          | sig0_20hz_c          | sig0_used_20hz_c == 0                    |
| Jason-3 Version F        | ku_sig0_ocean_mle3   | ku_sig0_ocean_mle3_compression_qual == 0 |
|                          | c_sig0_ocean         | c_sig0_ocean_compression_qual == 0       |
| Jason-3 Version T        |                      |                                          |
| Topex Version F          | sig0_20hz_ku_mle3    | sig0_used_20hz_ku == 0                   |
| Envisat Version 3        |                      |                                          |
| ERS-1 REAPER             |ocean_sig0_20hz       | ocean_sig0_used_20hz                     |
| ERS-2 REAPER             |ocean_sig0_20hz       | ocean_sig0_used_20hz                     |
| CryoSat-2  Version E     |                      |                                          |
| SARAL Version T          | sig0_40hz            | sig0_used_40hz == 0                      |
| Sentinel-6 A Version F08 | ku_sig0_ocean_mle3   | c_sig0_ocean_qual != 1                   |
|                          | c_sig0_ocean         | ku_sig0_ocean_mle3_qual != 1             |
| Sentinel-6 A Version F09 | ku_sig0_ocean_mle3   | c_sig0_ocean_qual != 1                   |
|                          | c_sig0_ocean         | ku_sig0_ocean_mle3_qual != 1             |
| Sentinel-3 A Version 005 | sig0_ocean_20_plrm_ku | sig0_ocean_qual_20_plrm_ku == 0          |
|                          | sig0_ocean_20_c      | sig0_ocean_qual_20_c == 0                |
| Sentinel-3 B Version 005 | sig0_ocean_20_plrm_ku |                                          |
|                          | sig0_ocean_20_c      |                                          |

### L2P Processing

#### Ancillary weather model output

#### Ancillary WW3 model output

