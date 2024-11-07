# Data workspaces

This sections describes the internal data workspace of CCI Sea State processing
system. This helps locating the different internal data produced by each 
processing step.

## Altimetry

### Retracked files

```{table} Location of the altimeter data retracked by CCI Sea State project
:name: retracked_workspace

| Mission       | Path                                                                                                                                                                                                                   |
|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JASON-1       | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/jason-1                                                                                                             |
| JASON-2       | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/jason-2                                                                                                             |
| JASON-3D      | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/jason-3d                                                                                                            |
| JASON-3F      | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/jason-3f                                                                                                            |
| SARAL         | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/saral                                                                                                               |
| CRYOSAT-2     | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/cryosat-2 & /home/datawork-cersat-public/provider/cci_seastate/processing/v4/altimeter/retracking/whales/cryosat-2/ |
| ERS-1         | /home/datawork-cersat-public/provider/esa/satellite/l2/ers-1/ra/esa-reaper/ers_alt_2_/data/date/                                                                                                                       |
| ERS-2         | /home/datawork-cersat-public/provider/esa/satellite/l2/ers-2/ra/esa-reaper/ers_alt_2_/data/date/                                                                                                                       |
| ENVISAT       | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/envisat                                                                                                             |
| TOPEX         | /home/datawork-cersat-public/provider/aviso/satellite/l2/topex-poseidon/topex/gdr/version_f/                                                                                                                           |
| SENTINEL-3A   | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/sentinel-3_a                                                                                                        |
| SENTINEL-3B   | /home/datawork-cersat-public/provider/eumetsat/satellite/l2/sentinel-3b/sral/BC005_Release/                                                                                                                            |
| SENTINEL-6A   | /home/datawork-cersat-public/provider/eumetsat/satellite/l2/sentinel-6a/poseidon-4/p4_2__lr/f08/                                                                                                                       |
```

### 1Hz files

```{table} Location of the intermediate 1 Hz compressed data processed from the full resolution retracked data (retracked by CCI Sea State projet or original provider)
:name: 1hz_workspace

| Mission         | Path                                                                                          |
|-----------------|-----------------------------------------------------------------------------------------------|
| JASON-1         | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/jason-1e         |
| JASON-2         | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/jason-2d         |
| JASON-3D        | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/jason-3d         |
| JASON-3F        | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/jason-3f         |
| SARAL           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/saral            |
| CRYOSAT-2       | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/cryosat-2e       |
| ERS-1           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/ers-1            |
| ERS-2           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/ers-2-reaper     |
| ENVISAT         | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/envisat-v3       |
| TOPEX           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/topexf           |
| SENTINEL-3A     | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/sentinel-3_a_005 |
| SENTINEL-3B     | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/sentinel-3_b_005 |
| SENTINEL-6A-F08 | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/sentinel-6_a_f08 |
| SENTINEL-6A-F09 | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/sentinel-6_a_f09 |
```

### L2P files

```{table} Location of the intermediate L2P data processed from internal 1 Hz data
:name: internal_l2p_workspace

| Mission         | Path                                                                                          |
|-----------------|-----------------------------------------------------------------------------------------------|
| JASON-1         | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/jason-1e         |
| JASON-2         | no data yet                                                                                   |                           
| JASON-3D        | no data yet                                                                                   |                           
| JASON-3F        | no data yet                                                                                   |
| SARAL           | no data yet                                                                                   |
| CRYOSAT-2       | no data yet                                                                                   |
| ERS-1           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/ers-1-reaper     |
| ERS-2           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/ers-2-reaper     |
| ENVISAT         | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/envisat-v3       |
| TOPEX           | no data yet                                                                                   |
| SENTINEL-3A     | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/sentinel-3_a_005 |
| SENTINEL-3B     | no data yet                                                                                   |
| SENTINEL-6A-F08 | no data yet                                                                                   |
| SENTINEL-6A-F09 | no data yet                                                                                   |
```

## SAR

### L2P files

```{table} Location of the intermediate L2P data
:name: internal_l2p_sar_workspace

| Mission         | Path                                                                                          |
|-----------------|-----------------------------------------------------------------------------------------------|
| SENTINEL-1A / EW | /home/datawork-cersat-public/provider/cci_seastate/processing/v4/sar/data/ew_dlr/sentinel-1_a/ |
| SENTINEL-1B / EW | /home/datawork-cersat-public/provider/cci_seastate/processing/v4/sar/data/ew_dlr/sentinel-1_b/ |
| SENTINEL-1A / IW | /home/datawork-cersat-public/provider/cci_seastate/processing/v4/sar/data/iw_dlr/sentinel-1_a/ |
| SENTINEL-1B / IW | /home/datawork-cersat-public/provider/cci_seastate/processing/v4/sar/data/iw_dlr/sentinel-1_b/ |
```

