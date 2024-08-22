# Processing baseline

## version 4.0

### Compression to 1 Hz

- full resolution (20/40 Hz) SWH and sigma0 values are flagged as land when 
  their distance to coast is **greater than 1000m**, based on the Goddard Space 
  Flight Center 1km resolution grid of distance to coast
- sigma0 read from SGDR, in C and Ku band (Ka for SARAL) - using MLE3 sigma0 
  when available

| mission | sigma0 | sigma0 quality |
| Jason-1 | sig0_20hz_ku |  sig0_used_20hz_ku == 0 |
| | sig0_20hz_c |  sig0_used_20hz_c == 0 |


- SWH range for uncompressed (20/40 Hz) values: [-0.5, 30]
