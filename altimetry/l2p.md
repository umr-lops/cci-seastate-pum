(l2p)=
# L2P

The altimeter L2P products are along-track files, usually corresponding to a 
satellite pass, processed from each mission data provider’s L1B product 
(usually referred to as SGDR) containing the waveforms. A retracking 
algorithm is applied to retrieve significant wave height from the instrument 
waveforms.

Additional post-processing is performed, like quality control, adjustment of 
significant wave height to a common reference, uncertainty estimation, etc… and
complementary variables are also computed or added from other model or satellite
sources. The content is fully consistent and standardised for each mission 
included in this dataset.

For altimeters, only the Ku band measurements are considered, when available 
(the only current exception being SARAL for which only Ka band is provided).

This section describes in detail the specific content of the {{cci_version}} L2P 
for Sea State CCI, configured as shown in the table 
{numref}`l2p_content_summary`, which can be used to locate appropriate 
information in this document. 

```{table} Summary description of the contents of a CCI Sea State L2P data product
:name: l2p_content_summary

| netCDF File Contents | Description                                                                                               |
|----------------------|-----------------------------------------------------------------------------------------------------------|
| **Coordinate variables**  | Information to permit locating data on non-orthogonal grids, as defined in {numref}`coordinate_variables` | 
| **Instrumental data record variables** | instrumental variables for 1st band altimeter (usually Ku) as defined in {numref}`l2p_variables_instrumental`                                          | 
| **Geophysical data record variables** | environmental variables for 1st band altimeter (usually Ku) as defined in {numref}`l2p_variables_environmental`                                          | 
| **Auxiliary data record variables** | auxiliary variables as defined in {numref}`l2p_variables_auxiliary`                                          | 
| **Global Attributes**  | A collection of required global attributes describing general characteristics of the file, as defined in section {numref}`global_attributes`  |
```

(l2p_variables_instrumental)=
## L2P instrumental data record format specification
The {numref}`table_l2p_variables_instrumental` provides an overview of the CCI 
Sea State L2P instrumental data record within a L2P file. In the following
sections, each variable within the L2P data file is described in detail.


```{table} Summary description of CCI Sea State L2P instrumental data records
:name: table_l2p_variables_instrumental

| Variable Name          | Description           | Units  |
|-----------------------|------------------------|--------|
| [sigma0](__l2p_sigma0) | Ku band backscatter coefficient, as calculated from the retracking | dB |
| [sigma0_rms](__l2p_sigma0_rms) | RMS of the Ku band backscatter coefficient, within 1Hz cells, of the 20 Hz measurements calculated from the retracking| dB |
| [sigma0_num_valid](__l2p_sigma0_num_valid) | number of valid points used to compute Ku band backscatter coefficient, within 1Hz cells, of the 20 Hz measurements calculated from the retracking | 1 |
```

(l2p_variables_environmental)=
## L2P geophysical data record format specification
The {numref}`table_l2p_variables_environmental` provides an overview of the CCI 
Sea State L2P environment (geophysical) data record within a L2P file. In the 
following sections, each variable within the L2P data file is described in detail.

```{table} Summary description of CCI Sea State L2P geophysical data records
:name: table_l2p_variables_environmental

| Variable Name          | Description           | Units  |
|-----------------------|------------------------|--------|
| [swh](__l2p_swh) | Hs as retrieved from the altimeter retracker [no calibration,  no editing], averaged over 1 Hz cells. | m |
| [swh_rms](__l2p_swh_rms) | RMS of the full resolution Hs measurements in `swh` variable, within 1Hz cells | m |
| [swh_num_valid](__l2p_swh_num_valid) | number of valid full resolution Hs measurements used to compute the Hs in `swh` variable, within 1Hz cells | 1 |
```

(l2p_variables_auxiliary)=
## L2P auxiliary data record format specification
The {numref}`table_l2p_variables_auxiliary` provides an overview of the CCI 
Sea State L2P environment (geophysical) data record within a L2P file. In the 
following sections, each variable within the L2P data file is described in detail.

