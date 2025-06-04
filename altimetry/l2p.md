---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
---

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
| **Geophysical data record variables** | environmental variables for 1st band altimeter (usually Ku) as defined in {numref}`l2p_variables_environmental`                                          | 
| **Instrumental data record variables** | instrumental variables for 1st band altimeter (usually Ku) as defined in {numref}`l2p_variables_instrumental`                                          | 
| **Auxiliary data record variables** | auxiliary variables as defined in {numref}`l2p_variables_auxiliary`                                          | 
| **Global Attributes**  | A collection of required global attributes describing general characteristics of the file, as defined in section {numref}`global_attributes`  |
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
| [swh](__l2p_swh) | Significant wave height, as retrieved from the altimeter retracker, averaged over 1 Hz cells, without bias correction. | m |
| [swh_rms](__l2p_swh_rms) | RMS of the full resolution Hs measurements in `swh` variable, within 1Hz cells | m |
| [swh_num_valid](__l2p_swh_num_valid) | number of valid full resolution Hs measurements used to compute the Hs in `swh` variable, within 1Hz cells | 1 |
| [swh_adjusted](__l2p_swh_adjusted) | Significant wave height, averaged over 1 Hz cells, with cross-mission bias correction. | m |
| [swh_denoised](__l2p_swh_denoised) | Significant wave height, averaged over 1 Hz cells, with cross-mission bias correction and denoising. | m |
| [swh_uncertainty](__l2p_swh_uncertainty) | Uncertainty of the significant wave height averaged over 1 Hz cells | m |
| [swh_quality_level](__l2p_swh_quality_level) | Quality level (from 0 - worst to 3 - best) of the significant wave height averaged over 1 Hz cells | code |
| [swh_rejection_flags](__l2p_swh_rejection_flags) | flag specifying the  editing criteria on which a 1 Hz significant wave height measurement was rejected (meaning its quality level is not set to “good”).  | bit mask |
| [swh_emd_noise](__l2p_swh_emd_noise) | high-frequency noise attached to swh_adjusted, from EMD filter | m |
| [swh_emd_imf1](__l2p_swh_emd_imf1) | first IMF attached to the denoising of swh_adjusted, from EMD filter | m |
| [swh_emd_uncertainty](__l2p_swh_emd_uncertainty) | uncertainty estimated by EMD filter| m |
```


(__l2p_swh)=
### `swh`

The **significant wave height (SWH)**, within 1 Hz cells, averaged from 
groups of full resolution 20 Hz (40 Hz for Saral, 18 Hz for Envisat) 
measurements calculated from the altimeter retracking, without any 
cross-mission bias correction.

For ERS-1, ERS-2, TOPEX, Sentinel-3 A & B and Sentinel-6, the 1 Hz 
measurements were estimated from the full resolution SWH measurements 
provided in the source Agency’s GDR & SGDR products. Refer to the processing 
details {numref}`__retracking` for the specific source used for these missions.

For Jason-1, Jason-2, Jason-3, Envisat, SARAL and CryoSat-2, a specific 
retracking was performed, using the WHALES nadir altimetry retracker 
selected by the CCI Sea State experts. For more details on the WHALES 
retracker, refer to the processing details {numref}`__whales`.

For all missions, the groups of full resolution measurements used to calculate 
the 1 Hz values are exactly the same as in the source Agency’s GDR & SGDR products.
Both CCI and Agency files can be compared one to one, have the same number 
of measurements, and the same latitude, longitude, time for each 1 Hz 
measurement. A minimal number of 6 (12 for SARAL) valid points is required 
to estimate a valid 1 Hz measurement. For more information on how the full 
resolution measurements are compressed into 1 Hz values, refer to the 
processing details {numref}`__compression`.

The `swh` variable in a L2P product follows the format shown in table {numref}`l2p_swh`.


```{table} CDL example description of **<span style="font-family:courier;">swh</span>** variable
:name: l2p_swh

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh`     | m (meter) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh[(,:]'| sed 's/[[:space:]]//'
```


(__l2p_swh_rms)=
### `swh_rms`


```{table} CDL example description of **<span style="font-family:courier;">swh_rms</span>** variable
:name: l2p_swh_rms

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh_rms`     | m (meter) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_rms

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_rms[(,:]'| sed 's/[[:space:]]//'
```



(__l2p_swh_num_valid)=
### `swh_num_valid`

The number of valid points used to compute the significant wave height (SWH), 
within 1 Hz cells, from the full resolution measurements calculated
from each altimeter waveform by the retracker.

The groups of full resolution SWH measurements used to calculate the 1 Hz 
values are exactly the same as in the source Agency’s GDR & SGDR products. Both
CCI Sea State and Agency files can be compared one to one, have the same 
number of measurements, and the same latitude, longitude, time for each 1 Hz
measurement.


```{table} CDL example description of **<span style="font-family:courier;">swh_num_valid</span>** variable
:name: l2p_swh_num_valid

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| int               | `swh_num_valid`     | 1 (dimensionless) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_num_valid

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_numval[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_swh_adjusted)=
### `swh_adjusted`

The **bias-corrected significant wave height**, in meters. The correction is 
based on cross-mission intercalibration as described in {numref}`__bias_correction`.


```{table} CDL example description of **<span style="font-family:courier;">swh_adjusted</span>** variable
:name: l2p_swh_adjusted

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh_adjusted`     | m (meter) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_adjusted

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_adjusted[(,:]'| sed 's/[[:space:]]//'
```


(__l2p_swh_denoised)=
### `swh_denoised`

The **bias-corrected and denoised significant wave height**, in meters.

```{important}
This is the recommended significant wave height variable to be used for most 
applications. 
```

A non-parametric denoising method based on Empirical Mode Decomposition (EMD, Huang
et al., 1998) and inspired by wavelet thresholding is applied to the 
variable `swh_adjusted` to estimate the denoised significant wave height 
(see Kopsinis and McLaughlin, 2009, Quilfen et al., 2018 and Quilfen and 
Chapron, 2019ab), as detailed in {numref}`__denoising`.


```{table} CDL example description of **<span style="font-family:courier;">swh_denoised</span>** variable
:name: l2p_swh_denoised

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh_denoised`     | m (meter) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_denoised

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_denoised[(,:]'| sed 's/[[:space:]]//'
```


(__l2p_swh_uncertainty)=
### `swh_uncertainty`

```{table} CDL example description of **<span style="font-family:courier;">swh_uncertainty</span>** variable
:name: l2p_swh_uncertainty

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh_uncertainty`     | m (meter) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_uncertainty

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_uncertainty[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_swh_quality_level)=
### `swh_quality_level`

Quality control of individual altimeter measurements is performed with checks
over the calculated 1 Hz values and ancillary variables. As a result, the 
estimated significant wave height comes with a quality level provided in the 
`swh_quality_level` variable. Its meaning is defined as described in 
{numref}`quality_levels`.

```{table} Definition of the quality levels of significant wave height measurements
:name: quality_levels

| value                             | meaning                         | description                                                                                                                                  |
|-----------------------------------|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| 0                                 | undefined                       | the measurement value is not defined or relevant (missing value, etc…), no quality check was applied.                                        |
| 1                                 | bad                             | the measurement was qualified as not usable after quality check.                                                                             |
| 2                                 | acceptable                      | the measurement may still be usable for some application or the quality check could not fully assess if it is a bad or good value (suspect). |
| 3 | good | the measurement is usable.                                                                                                                   |
```

For more details on how the quality level is set for a significant wave 
height measurement, refer to the processing details 
{numref}`__swh_quality_level`.

The `swh_quality_level` variable in a L2P product follows the format shown in 
table {numref}`l2p_swh_quality_level`.

```{table} CDL example description of **<span style="font-family:courier;">swh_quality_level</span>** variable
:name: l2p_swh_quality_level

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh_quality_level`     | bit code |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_quality_level

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_quality_level[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_swh_rejection_flags)=
### `swh_rejection_flags`

When SWH measurements were rejected as bad, the reason (quality test) for 
which they were rejected is reported in the related `swh_rejection_flags` 
variable. Refer to {numred}`editing` for details on the tests performed for 
the quality check of the measurements.

{numref}`__swh_quality_level` provides the meaning of each flag possibly raised, 
stored as a specific bit of an integer.


```{table} Definition of the rejection flags of significant wave height measurements
:name: swh_flags

| bit                             | meaning                         | 
|---------------------------------|---------------------------------|
| nb_of_valid_swh_too_low | the measurement was considered as invalid as there are indications of unsuitable waveforms for a proper SWH calculation. In particular there was no remaining 20 Hz values after all checks (such as distance to the closest shoreline) and outlier tests.|
| sea_ice | the measurement has possible ice contamination. The sea ice fraction is taken from an external source (such as the CCI Sea Ice microwave based daily maps). Sea ice contamination is defined as areas where the sea ice fraction is greater than a minimal threshold (corresponding to 10% of ice in the current configuration).|
| swh_validity | the SWH measurement was considered as invalid (out of the ]0, 30] meter range for instance).|
| swh_rms_outlier | the measurement was considered as invalid when the RMS of the SWH measurements used to estimate each 1 Hz SWH measurement was beyond the acceptable threshold for a given range of SWH. |
| outlier | the measurement was considered as invalid when performing the SWH outlier test, based on the neighbouring measurements within a 100 km window. |                                                                    
```

The `swh_rejection_flags` variable in a L2P product follows the format shown in 
table {numref}`l2p_swh_rejection_flags`.

```{table} CDL example description of **<span style="font-family:courier;">swh_rejection_flags</span>** variable
:name: l2p_swh_rejection_flags

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `rejection_flags`     | bit mask |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_rejection_flags

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_rejection_flags[(,:]'| sed 's/[[:space:]]//'
```


(__l2p_swh_emd_noise)=
### `swh_emd_noise`

The **high-frequency noise of SWH** returned by the application of the 
Empirical Mode Decomposition (EMD, Huang et al., 1998) to the bias-corrected 
significant wave height (`swh_adjusted`), in meters. See {numref}`__denoising` 
for more details on the SWH denoising.


```{table} CDL example description of **<span style="font-family:courier;">swh_emd_noise</span>** variable
:name: l2p_swh_emd_noise

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh_emd_noise`     | m (meter) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_emd_noise

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_emd_noise[(,:]'| sed 's/[[:space:]]//'
```


(__l2p_swh_emd_imf1)=
### `swh_emd_imf1`

The **first Intrinsic Mode Function of SWH** returned by the application of the
Empirical Mode Decomposition (EMD, Huang et al., 1998) to the bias-corrected 
significant wave height (`swh_adjusted`), in meters. See {numref}`__denoising` 
for more details on the SWH denoising.


```{table} CDL example description of **<span style="font-family:courier;">swh_emd_imf1</span>** variable
:name: l2p_swh_emd_imf1

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh_emd_imf1`     | m (meter) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_emd_imf1

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_emd_imf1[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_swh_emd_uncertainty)=
### `swh_emd_uncertainty`

The **SWH uncertainty** returned by the application of the
Empirical Mode Decomposition (EMD, Huang et al., 1998) to the bias-corrected 
significant wave height (`swh_adjusted`), in meters. See {numref}`__denoising` 
for more details on the SWH denoising.


```{table} CDL example description of **<span style="font-family:courier;">swh_emd_uncertainty</span>** variable
:name: l2p_swh_emd_uncertainty

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `swh_emd_uncertainty`     | m (meter) |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_swh_emd_uncertainty

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]swh_emd_uncertainty[(,:]'| sed 's/[[:space:]]//'
```


(l2p_variables_instrumental)=
## L2P instrumental data record format specification
The {numref}`table_l2p_variables_instrumental` provides an overview of the CCI 
Sea State L2P instrumental data record within a L2P file, mainly the 
altimeter backscatter in available sensing bands (Ku, C, and Ka for SARAL).
In the following sections, each variable within the L2P data file is described 
in detail.

```{note}
Altimeters do not have all the same sensing bands and some of these 
variables may therefore be missing for some missions. 
```

```{table} Summary description of CCI Sea State L2P instrumental data records
:name: table_l2p_variables_instrumental

| Variable Name          | Description           | Units  |
|-----------------------|------------------------|--------|
| [sigma0_ku](__l2p_sigma0_ku) | Ku band backscatter coefficient, as calculated from the retracking | dB |
| [sigma0_ku_rms](__l2p_sigma0_ku_rms) | RMS of the Ku band backscatter coefficient, within 1Hz cells, of the 20 Hz measurements calculated from the retracking| dB |
| [sigma0_ku_num_valid](__l2p_sigma0_ku_num_valid) | number of valid points used to compute Ku band backscatter coefficient, within 1Hz cells, of the 20 Hz measurements calculated from the retracking | 1 |
| [sigma0_ku_quality_level](__l2p_sigma0_ku_quality_level) | Quality level (from 0 - worst to 3 - best) of the Ku band sigma0 averaged over 1 Hz cells |  |
| [sigma0_ku_rejection_flags](__l2p_sigma0_ku_rejection_flags) | flag specifying the editing criteria on which a 1 Hz Ku band sigma0 measurement was rejected (meaning its quality level is not set to “good”).  |  |
| [sigma0_c](__l2p_c_sigma0) | C band backscatter coefficient, as calculated from the retracking | dB |
| [sigma0_c_rms](__l2p_sigma0_c_rms) | RMS of the C band backscatter coefficient, within 1Hz cells, of the 20 Hz measurements calculated from the retracking| dB |
| [sigma0_c_num_valid](__l2p_sigma0_c_num_valid) | number of valid points used to compute C band backscatter coefficient, within 1Hz cells, of the 20 Hz measurements calculated from the retracking | 1 |
| [sigma0_c_quality_level](__l2p_sigma0_c_quality_level) | Quality level (from 0 - worst to 3 - best) of the C band sigma0 averaged over 1 Hz cells |  |
| [sigma0_c_rejection_flags](__l2p_sigma0_c_rejection_flags) | flag specifying the editing criteria on which a 1 Hz C band sigma0 measurement was rejected (meaning its quality level is not set to “good”).  |  |

```

(__l2p_sigma0_ku)=
### `sigma0_ku`

The **Ku-band backscatter coefficients (sigma0)**, within 1 Hz cells, averaged from 
groups of full resolution 20 Hz (18 Hz for Topex) measurements calculated from 
the altimeter retracking, without any cross-mission bias correction. The Ku-band
sigma0 is only provided for Ku-band altimeters (excluding SARAL/AltiKa for instance).

The 1 Hz measurements were estimated from the full resolution sigma0 measurements 
provided in the source Agency’s GDR & SGDR products, including when SWH was 
estimated with the CCI Sea State selected retracker (WHALES). Refer to the processing 
details {numref}`__retracking` for the specific source used for each mission.

For all missions, the groups of full resolution measurements used to calculate 
the 1 Hz values are exactly the same as in the source Agency’s GDR & SGDR products.
Both CCI and Agency files can be compared one to one, have the same number 
of measurements, and the same latitude, longitude, time for each 1 Hz 
measurement. A minimal number of 6 valid points is required 
to estimate a valid 1 Hz measurement. For more information on how the full 
resolution measurements are compressed into 1 Hz values, refer to the 
processing details {numref}`__compression`.

The `sigma0_ku` variable in a L2P product follows the format shown in table 
{numref}`l2p_sigma0_ku`.


```{table} CDL example description of **<span style="font-family:courier;">sigma0_ku</span>** variable
:name: l2p_sigma0_ku

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sigma0_ku`     | dB |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_sigma0_ku

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sigma0_ku[(,:]'| sed 's/[[:space:]]//'
```


(__l2p_sigma0_ku_rms)=
### `sigma0_ku_rms`

(__l2p_sigma0_ku_num_valid)=
### `sigma0_ku_num_valid`

The number of valid points used to compute the Ku band backscatter coefficient
(sigma0), within 1 Hz cells, from the full resolution measurements calculated
from each altimeter waveform by the source Agency’s retracker.

The groups of full resolution sigma0 measurements used to calculate the 1 Hz 
values are exactly the same as in the source Agency’s GDR & SGDR products. Both
CCI Sea State and Agency files can be compared one to one, have the same 
number of measurements, and the same latitude, longitude, time for each 1 Hz
measurement.


```{table} CDL example description of **<span style="font-family:courier;">sigma0_ku_num_valid</span>** variable
:name: l2p_sigma0_ku_num_valid

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sigma0_ku_num_valid`     | 1 |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_sigma0_ku_num_valid

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sigma0_ku_num_valid[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_sigma0_ku_quality_level)=
### `sigma0_ku_quality_level`

```{table} CDL example description of **<span style="font-family:courier;">sigma0_ku_quality_level</span>** variable
:name: l2p_sigma0_ku_quality_level

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sigma0_ku_quality_level`     | enumerate |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_sigma0_ku_quality_level

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sigma0_ku_quality_level[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_sigma0_ku_rejection_flags)=
### `sigma0_ku_rejection_flags`

```{table} CDL example description of **<span style="font-family:courier;">sigma0_ku_rejection_flags</span>** variable
:name: l2p_sigma0_ku_rejection_flags

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sigma0_ku_rejection_flags`     | bit code |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_sigma0_ku_rejection_flags

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sigma0_ku_rejection_flags[(,:]'| sed 's/[[:space:]]//'
```


(l2p_variables_auxiliary)=
## L2P auxiliary data record format specification
The {numref}`table_l2p_variables_auxiliary` provides an overview of the CCI 
Sea State L2P environment (geophysical) data record within a L2P file. In the 
following sections, each variable within the L2P data file is described in detail.

```{table} Summary description of CCI Sea State L2P ancillary data records
:name: table_l2p_variables_auxiliary

| Variable Name          | Description           | Units  |
|-----------------------|------------------------|--------|
| [distance_to_coast](__l2p_distance_to_coast) | Distance to the nearest shoreline | m |
| [bathymetry](__l2p_bathymetry) | Water depth to sea floor | m |
| [sea_ice_fraction](__l2p_sea_ice_fraction) | Water depth to sea floor | 1 |
| [era5_tclw](__l2p_era5_tclw) | Total column cloud liquid water | kg m-2 |
| [era5_t2m](__l2p_era5_t2m) | 2 metre temperature             | K | 
| [era5_sst](__l2p_era5_sst) | Sea surface temperature         | K |
| [era5_u10](__l2p_era5_u10) | 10 metre U wind component       | m s-1 |
| [era5_v10](__l2p_era5_v10) | 10 metre V wind component       | m s-1 |
| [era5_sp](__l2p_era5_sp) | Surface pressure                | Pa |
| [era5_swh](__l2p_era5_swh) | Significant height of combined wind waves and swell | |
| [era5_pp1d](__l2p_era5_pp1d)       | Peak wave period    | s |
| [era5_p1ps](__l2p_era5_p1ps)       | Mean wave period based on first moment of swell     | s |
| [era5_p140121](__l2p_era5_p140121)    | Significant wave height of first swell partition    | m |
| [era5_p140122](__l2p_era5_p140122)    | Mean wave direction of first swell partition        | degree |
| [era5_mwp](__l2p_era5_mwp)        | Mean wave period  | s |
| [era5_mwd](__l2p_era5_mwd)        | Mean wave direction  | degree |
| [era5_shww](__l2p_era5_shww)       | Significant height of wind waves  | m |
| [era5_mdww](__l2p_era5_mdww) | Mean direction of wind waves  | degree |
| [era5_mpww](__l2p_era5_mpww) | Mean period of wind waves  | s |
| [ww3_hs](__l2p_ww3_hs)     | Significant height of wind and swell waves | m |
| [ww3_t02](__l2p_ww3_t02)   | Mean period T02 | s |
| [ww3_t0m1](__l2p_ww3_t0m1)  | Mean period T0m1 | s |
| [ww3_emb](__l2p_ww3_emb)  | Electromagnetic bias coefficient | 1|
| [ww3_fp](__l2p_ww3_fp)  | Wave peak frequency  | s-1 |
| [ww3_dir](__l2p_ww3_dir)   | Wave mean direction (from)  | degree |
| [ww3_skw](__l2p_ww3_skw)   | skewness of P(z,sx,sy=0)  | 1 |
| [ww3_qkk](__l2p_ww3_qkk)   | 2D wavenumber peakedness |m rad-1|
```


(__l2p_distance_to_coast)=
### `distance_to_coast`

The distance to the nearest coastline for each ocean measurement was extracted from the
Distance to Nearest Coastline grid at 0.01 degree resolution, provided by the NASA
Goddard Space Flight Center (GSFC) Ocean Color Group and available at:
http://www.pacioos.hawaii.edu/metadata/dist2coast_1deg.html

```{table} CDL example description of **<span style="font-family:courier;">distance_to_coast</span>** variable
:name: l2p_distance_to_coast

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `distance_to_coast`     | m |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_distance_to_coast

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]distance_to_coast[(,:]'| sed 's/[[:space:]]//'
```


(__l2p_bathymetry)=
### `bathymetry`

The same bathymetry source was used for all mission to get the ocean sea 
floor depth. We selected the 15 arc second General Bathymetric Chart of the 
Oceans (GEBCO), 2024 (https://doi.org/10.5285/1c44ce99-0a0d-5f4f-e063-7086abc0ea0f), 
available at: https://www.gebco.net.


```{table} CDL example description of **<span style="font-family:courier;">bathymetry</span>** variable
:name: l2p_bathymetry

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `bathymetry`     | m |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_bathymetry

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]bathymetry[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_sea_ice_fraction)=
### `sea_ice_fraction`

Sea ice concentration is provided as an ancillary variable but also used in 
the SWH editing procedure. We use an external sea ice concentration product 
to discard possibly ice contaminated measurements. Because no products 
provides a complete temporal coverage (missed acquisitions, non continuities 
between different microwave radiometer missions, infrequent updates of some 
datasets), we had to use different sources, as reported in {numref}`sea_ice`.

The sea ice concentration is expressed as a fraction.

```{table} CDL example description of **<span style="font-family:courier;">sea_ice_fraction</span>** variable
:name: l2p_sea_ice_fraction

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sea_ice_fraction`     | 1 |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_sea_ice_fraction

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sea_ice_fraction[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_era5_tclw)=
### `era5_tclw`

The total column cloud liquid water in the atmosphere, from ERA5 model 
reanalysis, in kg per m2.


```{table} CDL example description of **<span style="font-family:courier;">era5_tclw</span>** variable
:name: l2p_era5_tclw

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `era5_tclw`     | kg m-2 |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_era5_tclw

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]era5_tclw[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_era5_t2m)=
### `era5_t2m`

The air temperature at 2 meter height, from ERA5 model reanalysis, in Kelvin.


```{table} CDL example description of **<span style="font-family:courier;">era5_t2m</span>** variable
:name: l2p_era5_t2m

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `era5_t2m`     | K |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_era5_t2m

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]era5_t2m[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_era5_sst)=
### `era5_sst`

The sea surface temperature, from ERA5 model reanalysis, in Kelvin.


```{table} CDL example description of **<span style="font-family:courier;">era5_sst</span>** variable
:name: l2p_era5_sst

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `era5_sst`     | K |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_era5_sst

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]era5_sst[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_era5_u10)=
### `era5_u10`

The zonal wind speed at 10 meter height, from ERA5 model reanalysis, in meter 
per second. 


```{table} CDL example description of **<span style="font-family:courier;">era5_u10</span>** variable
:name: l2p_era5_u10

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `era5_u10`     | m s-1 |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_era5_u10

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]era5_u10[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_era5_v10)=
### `era5_v10`

The meridian wind speed at 10 meter height, from ERA5 model reanalysis, in 
meter per second. 

```{table} CDL example description of **<span style="font-family:courier;">era5_v10</span>** variable
:name: l2p_era5_v10

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `era5_v10`   | m s-1 |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_era5_v10

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]era5_v10[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_era5_sp)=
### `era5_sp`

The atmospheric pressure at sea level, from ERA5 model reanalysis, in Pascal. 

```{table} CDL example description of **<span style="font-family:courier;">era5_sp</span>** variable
:name: l2p_era5_sp

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `era5_sp`   | m s-1 |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_era5_sp

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]era5_sp[(,:]'| sed 's/[[:space:]]//'
```

(__l2p_era5_swh)=
### `era5_swh`

The significant height of combined wind waves and swell, from ERA5/WAM model 
reanalysis, in meters. Note that ERA5 WAM model assimilates altimeter data. For
a fully independent SWH estimate, use the WW3 SWH (see {numref}`ww3_swh`).

```{table} CDL example description of **<span style="font-family:courier;">era5_swh</span>** variable
:name: l2p_era5_swh

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `era5_swh`   | m  |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_era5_swh

!ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]era5wave_swh[(,:]'| sed 's/[[:space:]]//'
```

