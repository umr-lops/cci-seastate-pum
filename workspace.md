# Data workspaces

This sections describes the internal data workspace of CCI Sea State processing
system. This helps locating the different internal data produced by each 
processing step.

## Altimetry

### Full resolution (retracked) files

```{table} Location of the altimeter data retracked by CCI Sea State project (WHALES)
:name: retracked_workspace_cci

| Mission       | Path                                                                                                           |
|---------------|----------------------------------------------------------------------------------------------------------------|
| JASON-1 (E)   | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/jason-1e    |
| JASON-2 (D)   | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/jason-2     |
| JASON-3 (D)   | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/jason-3d    |
| JASON-3 (F)   | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/jason-3f    |
| ENVISAT (v3)  | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/envisat    |
| SARAL (F)     | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/saralf     |
| CRYOSAT-2 (F)  | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/cryosat-2/ |                                                                                                                     |
```

```{table} Location of the altimeter data retracked by third party
:name: retracked_workspace

| Mission       | Path     |
|---------------|----------|
| ERS-1 (REAPER) | /home/datawork-cersat-public/provider/esa/satellite/l2/ers-1/ra/esa-reaper/ers_alt_2_/data/date/                                                                                                                       |
| ERS-2 (REAPER) | /home/datawork-cersat-public/provider/esa/satellite/l2/ers-2/ra/esa-reaper/ers_alt_2_/data/date/                                                                                                                       |
| TOPEX (F)     | /home/datawork-cersat-public/provider/aviso/satellite/l2/topex-poseidon/topex/gdr/version_f/                                                                                                                           |
| SENTINEL-3A (BC005) | /home/datawork-cersat-public/provider/cci_seastate/products/v3/data/satellite/altimeter/l2_20Hz/l2/sentinel-3_a                                                                                                        |
| SENTINEL-3B  (BC005) | /home/datawork-cersat-public/provider/eumetsat/satellite/l2/sentinel-3b/sral/BC005_Release/                                                                                                                            |
| SENTINEL-6A (F08) | /home/datawork-cersat-public/provider/eumetsat/satellite/l2/sentinel-6a/poseidon-4/p4_2__lr/f08/                                                                                                                       |
```

### 1Hz files

```{table} Location of the intermediate 1 Hz compressed data processed from the full resolution retracked data (retracked by CCI Sea State projet or original provider)
:name: 1hz_workspace

| Mission         | Path                                                                                          |
|-----------------|-----------------------------------------------------------------------------------------------|
| JASON-1 (E)    | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/jason-1e          |
| JASON-2 (D)    | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/jason-2d          |
| JASON-3 (D)    | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/jason-3d          |
| JASON-3 (F)    | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/jason-3f          |
| SARAL (F)      | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/saralf            |
| CRYOSAT-2 (E)  | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/cryosat-2e        |
| ERS-1 (REAPER) | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/ers-1-reaper     |
| ERS-2 (REAPER) | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/ers-2-reaper     |
| ENVISAT (v3)   | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/envisat-v3       |
| TOPEX (F/TOPEX-A) | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/topexf_topex_a |
| TOPEX (F/TOPEX-B) | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/topexf_topex_b |
| SENTINEL-3A (BC005) | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/sentinel-3_a_005 |
| SENTINEL-3B (BC005) | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/sentinel-3_b_005 |
| SENTINEL-6A (F08) | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/1hz/sentinel-6_a_f08 |
```

### L2P files

```{table} Location of the intermediate L2P data processed from internal 1 Hz data
:name: internal_l2p_workspace

| Mission         | Path                                                                                          |
|-----------------|-----------------------------------------------------------------------------------------------|
| JASON-1         | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/jason-1e         |
| JASON-2         | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/jason-2d         |                           
| JASON-3D        | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/jason-3d         |                           
| JASON-3F        | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/jason-3f         |
| SARAL           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/saralf      |
| CRYOSAT-2       | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/cryosat-2e      |
| ERS-1           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/ers-1-reaper     |
| ERS-2           | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/ers-2-reaper     |
| ENVISAT         | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/envisat-v3       |
| TOPEX (F/TOPEX-A) |  /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/topexf_topex_a |
| TOPEX (F/TOPEX-B) |  /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/topexf_topex_b |
| SENTINEL-3A     | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/sentinel-3_a_005 |
| SENTINEL-3B     | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/sentinel-3_b_005 |
| SENTINEL-6A-F08 | /home/datawork-cersat-public/cache/project/cciseastate/data/v4/altimeter/l2p/sentinel-6_a_f08 |
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

