---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
mystnb:
  execution_timeout: 360
---

(l2p_wind)=
# L2P Wind

The altimeter L2P Wind products are along-track files, usually corresponding 
to a satellite pass, processed from each mission data provider’s L2 product 
(usually referred to as GDR) containing the 1 Hz backscatter values ($\sigma^0$), 
from which the altimeter wind is calculated after 
[intercalibrating these $\sigma^0$](__sigma0_bias_correction). 

It is completed with additional ancillary fields from ERA5 reanalysis and 
from the CCI Sea State L2P SWH to support the assessment of the retrieved 
wind or the development of new wind retrieval algorithms.

The content is fully consistent and standardised for each mission 
included in this dataset. Besides each file covers the same acquisition time 
period and the same number of measurements as its L2P SWH equivalent so that 
both L2P can be overlayed and combined together.

```{admonition} Status
:class: note
This dataset is considered as less mature than the L2P SWH. Besides, the 
altimeter wind is calculated from Ku-band $\sigma^0$ only; however the 
intercalibration method to bias correct the Ku-band $\sigma^0$ uses the 
difference of C-Band and Ku-band $\sigma^0$. Therefore the L2P wind dataset 
only applies to altimeters measuring both in C-band and Ku-band, and then to a 
more limited set of altimetry missions. Last, because of some changes in the 
$\sigma^0$ processing by the agencies providing the initial data, the method 
could not always be applied to the full temporal coverage of a given altimeter.

**Therefore the SWH and Wind L2P were separated in two different 
file collections to avoid misleading inconsistencies in the products content.**
```

This section describes in detail the specific content of the {{cci_version}} L2P 
Wind for Sea State CCI, configured as shown in {numref}`l2p_wind_content_summary`, 
which can be used to locate appropriate information in this document. 

```{table} Summary description of the contents of a CCI Sea State L2P Wind data product
:name: l2p_wind_content_summary

| netCDF File Contents | Description                                                                                               |
|----------------------|-----------------------------------------------------------------------------------------------------------|
| **Coordinate variables**  | Information to permit locating data on non-orthogonal grids, as defined in {numref}`coordinate_variables_all` | 
| **Geophysical data record variables** | environmental variables for Ku band altimeter as defined in {numref}`l2p_wind_variables_environmental`  | 
| **Instrumental data record variables** | instrumental variables for Ku and C band altimeter as defined in {numref}`l2p_wind_variables_instrumental`  | 
| **Auxiliary data record variables** | auxiliary variables as defined in {numref}`l2p_wind_variables_auxiliary`                                          | 
| **Global Attributes**  | A collection of required global attributes describing general characteristics of the file, as defined in section {numref}`global_attributes`  |
```

(l2p_wind_variables_environmental)=
## L2P geophysical data record format specification
{numref}`table_l2p_wind_variables_environmental` provides an overview of 
the CCI Sea State L2P environment (geophysical) data record within a L2P file. In the 
following sections, each variable within the L2P data file is described in detail.

```{table} Summary description of CCI Sea State L2P Wind geophysical data records
:name: table_l2p_wind_variables_environmental

| Variable Name          | Description           | Units  |
|-----------------------|------------------------|--------|
| [wind_speed_adjusted](__l2p_wind_speed_adjusted) | Wind speed, averaged over 1 Hz cells, with cross-mission bias correction. | m s-1 |
| [swh_adjusted](__l2p_swh_adjusted) | Significant wave height, averaged over 1 Hz cells, with cross-mission bias correction. | m |
| [swh_denoised](__l2p_swh_denoised) | Significant wave height, averaged over 1 Hz cells, with cross-mission bias correction and denoising. | m |
| [swh_quality_level](__l2p_swh_quality_level) | Quality level (from 0 - worst to 3 - best) of the significant wave height averaged over 1 Hz cells | code |
| [swh_rms](__l2p_swh_rms) | RMS of the full resolution Hs measurements in `swh` variable, within 1Hz cells | m |
| [swh_num_valid](__l2p_swh_num_valid) | number of valid full resolution Hs measurements used to compute the Hs in `swh` variable, within 1Hz cells | 1 |
```

(__l2p_wind_speed_adjusted)=
### `wind_speed_adjusted`

The wind speed, in m/s, computed from the [bias corrected altimeter radar backscatter $\sigma^0$](__sigma0_bias_correction)
using Abdalla (2012) relation.

```{code-cell}
:tags: [remove-input]
:name: l2p_wind_speed_adjusted

!bash -c "ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]wind_speed_adjusted[(,:]'| sed 's/[[:space:]]//'"
```


(l2p_wind_variables_instrumental)=
## L2P instrumental data record format specification
{numref}`table_wind_l2p_variables_instrumental` provides an overview of the CCI 
Sea State L2P instrumental data record within a L2P file, mainly the 
altimeter backscatter in available sensing bands (Ku, C, and Ka for SARAL).
In the following sections, each variable within the L2P data file is described 
in detail.

```{note}
Altimeters do not have all the same sensing bands and some of these 
variables may therefore be missing for some missions. 
```

```{table} Summary description of CCI Sea State L2P Wind instrumental data records
:name: table_wind_l2p_variables_instrumental

| Variable Name          | Description           | Units  |
|-----------------------|------------------------|--------|
| [sigma0_ku](__l2p_wind_sigma0_ku) | Ku band backscatter coefficient, as calculated from the source retracker | dB |
| [sigma0_c](__l2p_wind_sigma0_c) | C band backscatter coefficient, as calculated from the source retracker | dB |
| [sigma0_ku_adjusted](__l2p_wind_sigma0_ku_adjusted) | Ku band bias corrected backscatter coefficient | dB |
| [sigma0_c_adjusted](__l2p_wind_sigma0_c_adjusted) | C band bias corrected backscatter coefficient | dB |
| [agc_ku](__l2p_wind_agc_ku) | Ku band corrected Automatic Gain Control | dB |
| [agc_c](__l2p_wind_agc_c) | C band corrected Automatic Gain Control | dB |
```

(__l2p_wind_sigma0_ku)=
### `sigma0_ku`

The 1 Hz **Ku-band backscatter coefficient ($\sigma^0$)**, in dB, taken 
from the source Agency’s GDR  products. Refer to {numref}`sigma0_corrected_missions` 
for the exact variable in the GDR product that was used for each altimeter.

```{admonition} Status
:class: note

The `sigma0_ku` variable found in this product is different from the `sigma0_ku` 
ariable in the L2P SWH product which is calculated from the full resolution $\sigma^0$ 
possibly using a different retracker.
```

The `sigma0_ku` variable in a L2P product follows the format shown in table 
{numref}`l2p_wind_sigma0_ku`.


```{table} CDL example description of **<span style="font-family:courier;">sigma0_ku</span>** variable
:name: l2p_wind_sigma0_ku

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sigma0_ku`     | dB |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_wind_sigma0_ku

!bash -c "ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sigma0_ku[(,:]'| sed 's/[[:space:]]//'"
```

(__l2p_wind_sigma0_c)=
### `sigma0_c`

The 1 Hz **C-band backscatter coefficient ($\sigma^0$)**, in dB, taken 
from the source Agency’s GDR  products. Refer to {numref}`sigma0_corrected_missions` 
for the exact variable in the GDR product that was used for each altimeter.

```{table} CDL example description of **<span style="font-family:courier;">sigma0_c</span>** variable
:name: l2p_wind_sigma0_c

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sigma0_c`     | dB |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_wind_sigma0_c

!bash -c "ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sigma0_c[(,:]'| sed 's/[[:space:]]//'"
```

(__l2p_wind_sigma0_ku_adjusted)=
### `sigma0_ku_adjusted`

The 1 Hz **bias corrected Ku-band backscatter coefficient ($\sigma^0$)**, in dB.
Refer to {numref}`__sigma0_bias_correction` for more details on the correction method.


```{table} CDL example description of **<span style="font-family:courier;">sigma0_ku_adjusted</span>** variable
:name: l2p_wind_sigma0_ku_adjusted

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sigma0_c`     | dB |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_wind_sigma0_ku_adjusted

!bash -c "ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sigma0_c[(,:]'| sed 's/[[:space:]]//'"
```

(__l2p_wind_sigma0_c_adjusted)=
### `sigma0_c_adjusted`

The 1 Hz **bias corrected C-band backscatter coefficient ($\sigma^0$)**, in dB.
Refer to {numref}`__sigma0_bias_correction` for more details on the correction method.


```{table} CDL example description of **<span style="font-family:courier;">sigma0_c_adjusted</span>** variable
:name: l2p_wind_sigma0_c_adjusted

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `sigma0_c`     | dB |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_wind_sigma0_c_adjusted

!bash -c "ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]sigma0_c[(,:]'| sed 's/[[:space:]]//'"
```

(__l2p_wind_agc_ku)=
### `agc_ku`

The 1 Hz **Ku-band backscatter corrected automatic gain control (AGC)**, in dB, taken 
from the source Agency’s GDR  products. It is used for the bias correction of 
Ku-band $\sigma^0$.


```{table} CDL example description of **<span style="font-family:courier;">agc_ku</span>** variable
:name: l2p_wind_agc_ku

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `agc_ku`     | dB |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_wind_agc_ku

!bash -c "ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]agc_ku[(,:]'| sed 's/[[:space:]]//'"
```

(__l2p_wind_agc_c)=
### `agc_c`

The 1 Hz **C-band backscatter corrected automatic gain control (AGC)**, in dB, taken 
from the source Agency’s GDR  products. It is used for the bias correction of 
C-band $\sigma^0$.


```{table} CDL example description of **<span style="font-family:courier;">agc_c</span>** variable
:name: l2p_wind_agc_c

| **Storage type**  | **Name**  | **Unit** |
|-------------------|-----------|----------|
| float             | `agc_c`     | dB |
```

```{code-cell}
:tags: [remove-input]
:name: l2p_wind_agc_c

!bash -c "ncdump -h ../samples/ESACCI-SEASTATE-L2P-SWH-ERS-1-19950410T002419-fv01.nc | grep $'[ , \t]agc_c[(,:]'| sed 's/[[:space:]]//'"
```


(l2p_wind_variables_auxiliary)=
## L2P auxiliary data record format specification
{numref}`table_l2p_wind_variables_auxiliary` provides an overview of the CCI 
Sea State L2P auxiliary data records within a L2P file. In the 
following sections, each variable within this table is described in detail.

```{table} Summary description of CCI Sea State L2P ancillary data records
:name: table_l2p_wind_variables_auxiliary

| Variable Name          | Description           | Units  |
|-----------------------|------------------------|--------|
| [era5_sea_surface_temperature](__l2p_era5_sst) | Sea surface temperature         | K |
| [era5_eastward_wind](__l2p_era5_u10) | 10 metre U wind component       | m s-1 |
| [era5_northward_wind](__l2p_era5_v10) | 10 metre V wind component       | m s-1 |
```

