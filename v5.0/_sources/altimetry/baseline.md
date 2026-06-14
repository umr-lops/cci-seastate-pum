# Processing details

This section describes in details the processing workflow for the altimeter 
data in CCI Sea State {{cci_version}}, as illustrated in figure 
{numref}`altimetry_workflow`.

```{figure} ../images/altimetry_workflow.png
:name: altimetry_workflow

Main steps in altimeter data processing workflow
```

In a nutshell:
- Significant wave height is computed from the original altimeter 
  waveforms through the **retracking** step with the *WHALES* retracker 
  selected by CCI Sea State expert, though for some missions the values 
  produced by the space agencies default retracker are used.
- the full resolution SWH and sigma0 measurements are **compressed** to 1 Hz 
  (one value every second) to produce a L2 product
- **editing** of the 1 Hz values is performed to flag bad or suspect 
  values (and set the quality confidence variable `quality_level`)
- **ancillary variables** extracted from different sources (models and 
  observations, climatology, land, sea ice,...) are added into the L2 product.
- **bias correction** of SWH and backscatter ($\sigma^0$) is estimated for each 
  mission to ensure consistent time series of measurements across all missions, and a 
  corrected value added to the L2 product
- **denoising** is applied to the corrected SWH values using  Empirical Mode 
  Decomposition (EMD) filter and added to the L2P product as a new 
  `swh_denoised` variable
- **uncertainty** of SWH values is estimated and added to the L2P as a new 
  `swh_uncertainty` variable
- the **wind speed** is calculated from the cross calibrated $\sigma^0$ using the 
  same retrieval algorithm
- the L2P measurements are aggregated into multi-mission observation files 
  (**L3**) and monthly statistics of SWH (**L4**) are calculated


(__sgdr_inputs)=
## SGDR input data

The table {numref}`sgdr_inputs` summarizes the list of input altimeter data 
used in the processing workflow:

```{table} List of SGDR input data
:name: sgdr_inputs

| Mission     | Version     | Provider | Temporal coverage             |
|-------------|-------------|----------|-------------------------------|
| ERS-1       | REAPER      | ESA      | 03/08/1991 to 02/06/1996 |
| ERS-2       | REAPER      | ESA      | 14/05/1995 to 02/07/2003 |
| JASON-1     | Version E   | AVISO    | 15/01/2002 to 21/06/2013 |
| JASON-2     | Version D   | AVISO    | 04/07/2008 to 01/10/2019 |
| JASON-3     | Version F   | AVISO    | 12/02/2015 to 07/01/2025 |
| JASON-3     | Version G   | AVISO    | 30/01/2025 to 31/12/2025 |
| TOPEX-Poseidon | Version F   | AVISO    | 13/10/1992 to 04/10/2005 |
| ENVISAT     | Version 3   | ESA      | 14/05/2002 to 08/04/2012 |
| GFO         |             | NOAA     | 08/01/2000 to 17/09/2008 |
| CRYOSAT-2   | Version E   | ESA      | 16/07/2010 to 31/12/2025 |
| SARAL       | Version F   | AVISO    | 14/03/2013 to 31/12/2025 |
| Sentinel-3A | Version 005 | EUMETSAT | 01/07/2016 to 08/11/2024 |
| Sentinel-3A | Version G61 | EUMETSAT | 08/11/2024 to 31/12/2025 |
| Sentinel-3B | Version 005 | EUMETSAT | 08/05/2018 to 07/11/2024 |
| Sentinel-3B | Version G61 | EUMETSAT | 08/11/2024 to 31/12/2025 |
| Sentinel-6A | Version G01 | EUMETSAT | 17/12/2020 to 31/12/2025 |
| SWOT Nadir  | Version S02 | AVISO    | 16/02/2023 to 31/12/2025 |
| CFOSAT Nadir| Version OP06| AVISO    | 19/04/2019 to 09/10/2024 |
| CFOSAT Nadir| Version OP07| AVISO    | 09/10/2024 to 31/12/2025 |
```

```{admonition} Note on TOPEX-POSEIDON
According to TOPEX version F documentation:
- From launch through repeat cycle 16, various changes to the sensors were 
  performed. Data up to repeat cycle 16 have varying quality and should not be 
  used for climate studies. They are ignored in CCI Dataset.
- TOPEX Side A was active from September 22, 1992 to February 10, 1999, and 
  Side B was active from February 10, 1999 to end of mission. Users are advised 
  to treat the data from Side A and Side B as two independent time series. 
  No effort has been made to enforce continuity between Side A and Side B in 
  this reprocessed data. They are separated in CCI processing.
- TOPEX Side A calibration data have a jump on April 1, 1996. This very likely 
  causes a jump in all TOPEX altimeter geophysical measurements at this time. 
  Users are advised to treat data from Side A1 (Launch – April 1, 1996) and Side
  A2 (April 1, 1996 – February 10, 1999) as two independent time series. 
  No effort has been made to enforce continuity between Sides A1 and A2.  They 
  are separated in CCI processing.
- Sweep Calibration measurements to monitor the TOPEX point target response only
  started from September 8, 1998 onward for both Side A and Side B. Users are 
  cautioned that this may cause larger errors in the reprocessed Side A data. 
  The Sweep Calibration data available for Side A from September 8, 1998 to 
  February 10, 1999 are not used by themselves to process the Side A data in 
  this product. Instead, for Side A, the available Sweep Calibrations have been 
  used to generate a model for oversampled calibrations. The nominal Cal-1 data 
  are used with this model to process all of Side A data to generate a 
  consistent Side A time series.
- Three 8-track tape recorders (TR A, B, C) were utilized to continuously record
  the 16K data stream, acquiring over 99.9% of all spacecraft and science data. 
  However, after 5 years of excellent performance the recorders, starting with 
  TRB, slowly started to degrade. Work arounds involving recording and playback 
  speed restored some performance for periods. TRB was deactivated in September 
  2001 and TRA in October 2002. Real time acquisition through TDRSS filled much 
  of the loss allowing approximately 90% data coverage. Finally, in October 2004
  TRC failed. Real time data acquisition provided approximately 82% data 
  coverage until the end of the mission.
```


(__whales)=
## Retracking

The retracking step calculates 20 Hz SWH measurements (40 Hz for Saral) from 
the altimeter waveforms available in the SGDR products (Level 1) provided by 
space agencies. In CCI SeaState version {{cci_version}}, we have 
performed a complete retracking of several non-SAR altimeters with TUM’s 
retracker WHALES, while for others we used the full resolution SWH 
measurements already processed by the space agencies (see 
{numref}`swh_retrackers` for a summary). Reasons why some altimeters were 
not retracked with WHALES in this release include: non availability of the 
waveforms, non applicability of WHALES to an altimeter (e.g. SAR altimeters, 
lack of technical information to adapt the retracker), or postponement to a 
future release (CFOSAT/Nadir, ...). 

The Low Resolution Mode (LRM) waveforms are characterised by a rising leading edge that
becomes less steep as the SWH increases, and a slowly decreasing trailing edge. The
standard retracking methods are still affected by a suboptimal distribution of the residuals in
the fitting process, which results in high level of noise in the estimations. WHALES is
designed as a unified way to solve these problems and is based on two principles:
1. The application of a weighted fitting solution, whose weights are adapted 
   to the SWH in order to guarantee a more uniform distribution of the 
   residuals during the iterative fitting. This guarantees significantly 
   more precise estimations.
2. A subwaveform strategy to focus the retracking on the portion of the signal 
   of interest, avoiding heterogeneous backscattering in the trailing edge 
   (partially inherited from the ALES retracker, Passaro et al., 2014). This 
   guarantees efficiency in the coastal zone and a better representation of 
   the oceanic scales of variability.

Moreover, a revisiting of the look-up tables used to correct for the 
Gaussian approximation of the Point Target Response in the Brown model 
ensures that the accuracy in the estimation is tailored to the new retracking 
solution.

```{admonition} References
:class: note

Passaro M., Cipollini P., Vignudelli S., Quartly G., Snaith H.: ALES: A multi-mission
subwaveform retracker for coastal and open ocean altimetry. Remote Sensing of
Environment 145, 173-189, https://doi.org/10.1016/j.rse.2014.02.008, 2014
```

{numref}`swh_retrackers` summarized the retracker used for each 
mission for significant wave height, whether the retracking was performed by 
CCI Sea State production team (WHALES) or a third party agency:

```{table} Retracker used for each mission for retrieving the significant wave height
:name: swh_retrackers

| source                   | period             | retracker     | 
|--------------------------|--------------------|---------------|
| ERS-1                    | 07/1991 to 03/2000 | WHALES        | 
| ERS-2                    | 04/1995 to 07/2011 | WHALES        | 
| Jason-1 Version E        | 01/2002 to 07/2013 | WHALES        | 
| Jason-2 Version D        | 06/2008 to 10/2019 | WHALES        | 
| Jason-3 Version F        | 02/2016 to 01/2025 | WHALES        | 
| Jason-3 Version G        | 01/2025 to 12/2025 | WHALES        | 
| Topex Version F          | 08/1992 to 01/2006 | MLE3          |
| GFO                      | 01/2000 to 09/2008 |           |
| Envisat Version 3        | 03/2002 to 04/2012 | WHALES        | 
| CryoSat-2  Version E     | 07/2010 to now     | WHALES        | 
| SARAL Version F          | 02/2013 to now     | WHALES        |
| Sentinel-6 A Version G   | 03/2020 to 12/2023 | WHALES          |   
| Sentinel-3 A Version 005 | 02/2016 to now     | MLE4          |
| Sentinel-3 B Version 005 | 04/2018 to now     | MLE4          |
| SWOT Nadir | 02/2023 to 12/2025 | WHALES          |   
| CFOSAT OP06 | 04/2019 to 10/2024 | Adaptive  |   
| CFOSAT OP07 | 10/2024 to 12/2025 | Adaptive  |   
```

For sigma0, the measurements from the original retracking performed by the agency 
which provided the input data (SGDR) were used, with no correction by CCI Sea 
State for this version. {numref}`sigma0_retrackers` summarized the retracker 
used for each mission for sigma0 by these agencies:

```{table} Retracker used for each mission for retrieving the sigma0
:name: sigma0_retrackers

| source                   | period             | retracker (per band)  | 
|--------------------------|--------------------|-----------------------|
| ERS-1                    | 07/1991 to 03/2000 | REAPER/MLE3 (Ku)      | 
| ERS-2                    | 04/1995 to 07/2011 | REAPER/MLE3 (Ku)      | 
| Jason-1 Version E        | 01/2002 to 07/2013 | MLE3 (Ku, C)          | 
| Jason-2 Version D        | 06/2008 to 10/2019 | MLE3 (Ku, C)          | 
| Jason-3 Version F        | 02/2016 to 01/2025 | MLE3  (Ku, C)         | 
| Jason-3 Version G        | 01/2025 to 12/2025 | MLE3  (Ku, C)         | 
| Topex Version F          | 08/1992 to 01/2006 | MLE3 (Ku, C)          |
| Envisat Version 3        | 03/2002 to 04/2012 | MLE3 (C)              | 
| CryoSat-2  Version E     | 07/2010 to now     | Ocean CFI/MLE4 (Ku)   |
| SARAL Version F          | 02/2013 to now     | MLE4 (Ka)             |
| Sentinel-6 A Version G01 | 03/2020 to 12/2025 | MLE3 (Ku, C)     ?     |   
| Sentinel-3 A Version 005 | 02/2016 to ?     | MLE4 (Ku), MLE3 (C)   |
| Sentinel-3 A Version G61 |     | MLE4 (Ku), MLE3 (C)  ? |
| Sentinel-3 B Version 005 | 04/2018 to ?     | MLE4 (Ku), MLE3 (C)   |
| SWOT Nadir | 02/2023 to 12/2025 | ?          |   
| CFOSAT OP06 | 04/2019 to 10/2024 | Adaptive    ?      |   
| CFOSAT OP07 | 04/2019 to 10/2024 | Adaptive     ?     |   
```


(__compression)=
## Compression to 1 Hz

The CCI Sea State Dataset {{cci_version}} provides 1 Hz SWH measurements. 
These 1 Hz measurements are calculated by averaging groups of consecutive 
full resolution 20 Hz (18 Hz for Envisat or Topex, 40 Hz for SARAL). 

The method used to average the full resolution measurements into 1 Hz values is 
the same for all altimeters. The groups of full resolution measurements used to
calculate the 1 Hz values are exactly the same as in the source Agency’s GDR 
and SGDR products. Both CCI and Agency files can be compared one to one, have 
the same number of measurements, and the same latitude, longitude, time for each
1 Hz measurement.

For each group of valid (the measurements remaining after the editing 
described above) measurements, the 1 Hz SWH value is computed as the median 
of the remaining full resolution SWH values and copied to `swh` variable. 
Moreover, the `swh_num_valid` variable is computed as the number of 
remaining valid full resolution measurements and the `swh_rms` variable is 
computed as the root mean square of the deviation from the median,
considering only valid full resolution measurements.

The same method is used to calculate 1 Hz sigma0 values from the full 
resolution sigma0.

The compression of the full resolution (20/40 Hz) SWH and sigma0 
measurements into 1 Hz values follows these steps:

1. Full resolution (20/40 Hz) SWH and sigma0 values are flagged as **land** 
   when their distance to coast is **greater than 1000m**, based on the Goddard 
   Space Flight Center (GSFC) 1km resolution grid of distance to coast. **They 
   are ignored in the compression process.**
2. Full resolution measurements flagged as bad by the retracker are discarded. 
   These measurements are flagged as bad, for altimeters retracked with WHALES, 
   when the fitting error is greater than 0.3. In Agency's SGDR, a quality 
   variable is usually associated with the SWH and sigma0 variables and used 
   as a substitute here. {numref}`fullres_swh` and {numref}`fullres_sigma0` 
   provides, respectively for SWH and sigma0, the source variable used as 
   input for the full resolution measurements, the associated quality 
   variable used for this editing and condition tested to discard the 
   measurements.
3. Full resolution SWH measurements **not in the -0.5 to 30 meter range** are 
   also discarded. 
4. Among the remaining full resolution SWH measurements, **outliers** are 
   discarded. The outlier detection scheme is based on the **maximum absolute 
   deviation (MAD)**  for a 3-sigma criterion, meaning:
   * only 20 Hz SWH values within [median(SWH) - 3 * MAD(SWH), median(SWH) + 3 * MAD(SWH)] interval are kept
   * with: MAD(SWH) = 1.4286 * median(abs(SWH - median(SWH)))
5. The **median** of the remaining measurements is selected as 1 Hz value. When 
   there were less than 6 (12 for SARAL) valid remaining measurements, the 1 
   Hz value is flagged as **bad** in the `quality_level` variable. 

```{table} selected variable for SWH in each full resolution dataset, and the corresponding quality flag variable used to discard invalid measurements
:name: fullres_swh

| Source                  | SWH                    | SWH quality                         |
|-------------------------|------------------------|-------------------------------------|
| Jason-1 Version E       | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| Jason-2 Version D       | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| Jason-3 Version F       | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| Jason-3 Version G       | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| Topex Version F         | swh_20hz_ku            | swh_used_20hz_ku == 0               |
| Envisat Version 3       | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| ERS-1 REAPER            | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| ERS-2 REAPER            | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| CryoSat-2 Version E     | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| Saral Version F         | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |                           
| Sentinel-6A Version G01 | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| Sentinel-3A Version 005 | swh_ocean_20_plrm_ku   | swh_ocean_qual_20_plrm_ku == 0      |
| Sentinel-3A Version G61 | swh_ocean_20_plrm_ku   | swh_ocean_qual_20_plrm_ku == 0      |
| Sentinel-3B Version 005 | swh_ocean_20_plrm_ku   | swh_ocean_qual_20_plrm_ku == 0      |
| Sentinel-3A Version G61 | swh_ocean_20_plrm_ku   | swh_ocean_qual_20_plrm_ku == 0      |
| SWOT Nadir version S02  | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           | 
| CFOSAT OP06             | swh                    | flag_valid_swh_1Hz == 0             |   
| CFOSAT OP07             | swh                    | flag_valid_swh_1Hz == 0             |   
```

For the compression of the **sigma0**, the processing is the same. Full 
resolution sigma0 measurements are discarded if not in the range: [7, 30] dB 
(except for SARAL Ka sigma0: [-6.5, 23.]).

```{important}
sigma0, in C and Ku band (Ka for SARAL), are **always taken from the 
agency's SGDR, even when using WHALES to estimate SWH** using MLE3 Ku band 
sigma0 when available, as summarized in {numref}`fullres_sigma0`.
```

```{table} selected variable for sigma0 in each full resolution dataset, for each sensing band, and the corresponding quality flag variable used to discard invalid measurements
:name: fullres_sigma0
| Source                   | Sigma0                | Sigma0 quality                           |
|--------------------------|-----------------------|------------------------------------------|
| Jason-1 Version E        | sig0_20hz_ku          | sig0_used_20hz_ku == 0                   |
|                          | sig0_20hz_c           | sig0_used_20hz_c == 0                    |
| Jason-2 Version D        | sig0_20hz_ku_mle3     | sig0_used_20hz_ku_mle3 == 0              |
|                          | sig0_20hz_c           | sig0_used_20hz_c == 0                    |
| Jason-3 Version F        | sig0_ocean_mle3       | sig0_ocean_mle3_compression_qual == 0    |
|                          | sig0_ocean            | sig0_ocean_compression_qual == 0         |
| Jason-3 Version G        | sig0_ocean_mle3       | sig0_ocean_mle3_compression_qual == 0    |
|                          | sig0_ocean            | sig0_ocean_compression_qual == 0         |
| Topex Version F          | sig0_20hz_ku_mle3     | sig0_used_20hz_ku == 0                   |
| Envisat Version 3        | sig0_ocean_20_ku      | sig0_ocean_qual_20_ku == 0               |
| ERS-1 REAPER             | ocean_sig0_20hz       | ocean_sig0_used_20hz                     |
| ERS-2 REAPER             | ocean_sig0_20hz       | ocean_sig0_used_20hz                     |
| CryoSat-2  Version E     | sig0_1_20_ku          |                                          |
| SARAL Version F          | sig0_40hz             | sig0_used_40hz == 0                      |
| Sentinel-6 A Version F08 | ku_sig0_ocean_mle3    | ku_sig0_ocean_mle3_qual != 1             |
|                          | c_sig0_ocean          | c_sig0_ocean_qual != 1                   |
| Sentinel-3 A Version 005 | sig0_ocean_20_plrm_ku | sig0_ocean_qual_20_plrm_ku == 0          |
|                          | sig0_ocean_20_c       | sig0_ocean_qual_20_c == 0                |
| Sentinel-3 B Version 005 | sig0_ocean_20_plrm_ku | sig0_ocean_qual_20_plrm_ku == 0          |
|                          | sig0_ocean_20_c       | sig0_ocean_qual_20_c == 0                |
| SWOT Nadir version S02   | sig0_ocean_mle3       | sig0_ocean_mle3_compression_qual == 0    |
|                          | sig0_ocean            | sig0_ocean_compression_qual == 0         | 
| CFOSAT OP06              | sigma0                | flag_valid_sigma0_1Hz == 0               |   
| CFOSAT OP07              | sigma0                | flag_valid_sigma0_1Hz == 0               |   
```

> **S-band sigma0** were ignored for **Envisat** as it was found they were 
> systematically flagged as bad in the SGDR product.


(__swh_quality_level)=
## Editing

The quality level of SWH measurement, provided in the `quality_level` variable 
is estimated by applying a suite of specific tests. When positive, a test will 
downgrade a SWH measurement (starting initially with the quality level inherited
from the 1 Hz compressed data file) to a lower value, as given in the following 
table. The result of each applied test is summarized in the corresponding 
`rejection_flags` variable.

```{table} Quality level assigned to a validity test if positive (with corresponding label in the rejection_flags variable
:name: swh_quality_level

| Validity test | Assigned quality level |
| ------------- | ---------------------- |
| `sea_ice`: 0 < sea ice concentration <= 10% | 2 |
| `sea_ice`: sea ice concentration > 10% | 1 |
| `swh_validity`: 0 <= SWH <= 30 | 1 |
| `swh_rms_outlier`: SWH RMS in 1 Hz meaurement > LUT value | 1 |
| `outlier_test`: SWH outlier test | 1 |
```

(__swh_rms_test)=
### Test on SWH RMS (`swh_rms_outlier`)

As documented in Sepulveda et al. (2015), SWH measurements derived from 
radar altimeter measurements can be contaminated by the presence of land in 
the footprint, strong rain events, and so-called “sigma0 blooms” due to weak 
winds or surface slicks. According to these authors, the standard deviation 
of the full resolution (20Hz/40Hz) SWH values over one second (hereinafter 
called SWH RMS) is one of the most relevant parameter to detect erroneous 
values of SWH. Since the SWH RMS level strongly depends on SWH, constant 
threshold values are not adequate to efficiently remove SWH RMS outliers.

Therefore, Sepulveda et al. (2015) and Queffeulou (2016) proposed a 
methodology to set a statistical threshold on SWH RMS that depends on SWH, 
and which can be used a posteriori to filter out erroneous SWH measurements. 
Since the SWH RMS for a given narrow SWH presents a log-normal distribution, 
these authors proposes to estimate the upper threshold for the logarithm of 
SWH RMS as the sum of the mean value and thrice the standard deviation. 
Moreover, in order to reduce discontinuity in the SWH RMS threshold function 
for large SWH values, where the number of records is too low to derive 
robust statistics, a second-order polynomial function is fitted. 

The methodology implemented to determine a SWH RMS LUT for each mission of 
the Sea State CCI {{ cci_version }} dataset can be described as follows: 

1. for each cycle of the mission duration, SWH and SWH RMS measurements are 
   sampled and invalid measurements are rejected based on the land mask and SWH 
   range quality flags; 
2. the mean and standard deviation of log(SWH RMS) are computed for SWH bins of
   0.5 m width, ranging from 0 to 15 m, with a 0.05 m increment. Only bins with
   more than 100 values are considered; 
3. upper threshold on SWH RMS are computed for each SWH bin as : $\exp(mean(\log{}swh\_rms)+3\times std(\log{}swh\_rms))$;
4. a second-order polynomial function is fitted to the SWH RMS threshold 
   function for SWH values comprised between 3 and 10m and is extrapolated 
   up to SWH = 12m;
5. a look-up table (LUT) of SWH RMS thresholds is derived by combining for 
   each cycle the SWH RMS threshold values for SWH lower than 3m and the 
   values of the polynomial function for SWH higher than 3m; for SWH above 
   12 m, a constant value equal to the SWH RMS threshold at 12m is taken;
6. the final LUT is computed as the ensemble mean of the N cycle LUTs;

Note that the selection of the lower and upper bounds (3-10m) used to 
estimate the second-order polynomial function are mission specific, and is 
determined for each method from visual inspection of the SWH_RMS = f(SWH) 
function.

A significant improvement with respect to the Sea State CCI dataset version 4 is 
the fact that the ensemble average LUT is now computed over the full mission 
duration and not on a sub sample of 8 cycles, as was done before.

(__swh_outlier_test)=
### Test on SWH outliers (`outlier_test`)

Above quality flags and tests are not sufficient to discard all the 
erroneous SWH data. Spurious measurements are still observed: some are 
located in the vicinity of the coast where some land can be within the 
altimeter footprint, or in areas of high scattering resulting in so-
called sigma0 blooms (e. g. Thibaut et al. 2007). Some other individual 
spurious measurements (corresponding mainly to high values of SWH) are not 
explained. Consequently the data are filtered to eliminate these measurements.

The screening is based on the analysis of the differences between successive 
along track SWH measurements, using  along track running windows, 100 km 
wide. For each measurement the along track data within 50 km each
side are selected. This represents a maximun number of 15 (Envisat) to 19 
(Jason) selected data. Then over this segment the 2 extreme SWH data are 
discarded, and the mean value (m) and standard deviation (s) are estimated 
over the residual data set. If the SWH value is outside the interval defined 
by m ± 4s, then this data is considered as erroneous and is discarded. Up
to 3 iterative passes were empirically adjusted for the processing.


## Ancillary data

The base L2 compressed files are enriched with additional external sources 
information.

### Ancillary atmospheric model variables

Complementary ocean or atmospheric variables are taken from ECMWF ERA5 
reanalysis, using spatial and temporal linear interpolation at measurement 
point.  

```{table} ERA5 atmospheric model variables added to each 1 Hz measurement in L2P
:name: ancillary_era5

| Variable | Description                     |
|----------|---------------------------------|
| tclw     | Total column cloud liquid water |
| t2m      | 2 metre temperature             |
| sst      | Sea surface temperature         |
| u10      | 10 metre U wind component       |
| v10      | 10 metre V wind component       |
| sp       | Surface pressure                |
```

### Ancillary wave model variables

Complementary sea state variables are taken from ECMWF ERA5 reanalysis (WAM 
model for waves) and Ifremer Wave Watch 3 hindcast run, using spatial and 
temporal linear interpolation at measurement point.


```{table} ERA5/WAM wave model variables added to each 1 Hz measurement in L2P
:name: ancillary_era5wam

| Variable   | Description                                         |
|------------|-----------------------------------------------------|
| swh        | Significant height of combined wind waves and swell |
| pp1d       | Peak wave period                                    |
| p1ps       | Mean wave period based on first moment of swell     |
| swh1       | Significant wave height of first swell partition    |
| mwd1       | Mean wave direction of first swell partition        |
| mwp        | Mean wave period                                    |
| mwd        | Mean wave direction                                 |
| shww       | Significant height of wind waves                    |
| mdww       | Mean direction of wind waves                        |
| mpww       | Mean period of wind waves                           |
```

```{table} Ifremer/WW3 hindcast wave model variables added to each 1 Hz measurement in L2P
:name: ancillary_ifrww3

| Variable   | Description                                |
|------------|--------------------------------------------|
| uwnd       | 10 metre U wind component                  |
| vwnd       | 10 metre V wind component                  |
| hs         | Significant height of wind and swell waves |
| t02        | Mean period T02                            |
| t0m1       | Mean period T0m1                           |
| 1/fp       | Wave peak frequency                        |
| dir        | Wave mean direction                        |
| skw        | skewness                                   | 
| qkk        | k-peakedness                               |
```

```{note}
The WW3 model configuration ran for this ancillary source uses surface currents 
(from CMEMS) and icebergs computed from altimetry and only available from 1993.
The configuration used for the years 1991-1992 is therefore different and not
full consistent with the model configuration used from 1993 onward. 
```

(__sea_ice)=
### Ancillary sea ice concentration

Different sources are combined for sea ice concentration, as the best 
resolution datasets (25 km) do not cover the full CCI Sea State temporal 
coverage. For each source, we use the closest in time concentration map, up to 
three days apart in case it is missing for a given day, before switching to the 
next source in line.

```{table} sources for sea ice concentration (SIC) CDR, by order of priority
:name: ancillary_sic

|  Product             | Variable | Temporal Coverage     | Description |
|----------------------|----------|-----------------------|-------------|
| OSISAF-438 | ice_conc | 2021-onward | AMSR-2 Interim Sea Ice Concentration Climate Data Record from MetNo | 
| OSI-458    | ice_conc | 2013-2020   | AMSR Sea Ice Concentration Climate Data Record from OSI SAF (doi: 10.15770/EUM_SAF_OSI_0015) |
| SICCI-HR-SIC  | ice_conc | 1991-2020  | High(er) Resolution Sea Ice Concentration Climate Data Record Version 3 from CCI Sea Ice+ (SSM/I and SSMIS) (doi: 10.5285/eade27004395466aaa006135e1b2ad1a) |
| OSISAF-450-a1 | ice_conc  | 1990-1991 | Sea Ice Concentration Climate Data Record Version 3 (SMMR, SSM/I, and SSMIS) from the EUMETSAT OSI SAF (doi: 10.15770/EUM_SAF_OSI_0023) |
```

### Bathymetry

The bathymetry depth is taken from the 15 arc-second resolution General 
Bathymetric Chart of the Oceans (GEBCO), 2024 (
https://doi.org/10.5285/1c44ce99-0a0d-5f4f-e063-7086abc0ea0f) which can be 
found at: https://www.gebco.net/. 

It is resampled on each L2P measurement location using closest neighbour.


### Distance to coast
 
The distance to closest shoreline is taken from the gridded data set of 
distances from the nearest coastline available at PacIOOS 
(https://www.pacioos.hawaii.edu/metadata/dist2coast_1deg.html) . Negative 
distances represent locations over land (including land-locked bodies of 
water), while positive distances represent the ocean. NASA's Ocean Biology 
Processing Group (OBPG) generated this data set using the Generic Mapping 
Tools (GMT) software package. Distances were computed with GMT using its 
intermediate-resolution coastline and then gridded globally at a spatial 
resolution of 0.04 degrees. Bilinear interpolation was then applied to 
increase the spatial resolution to 0.01 degrees. There is an uncertainty of 
up to 1 km in the computed distance at any given point.

It is resampled on each L2P measurement location using closest neighbour.


(__bias_correction)=
## SWH bias correction

### General organization of the method
The bias correction method implemented for the Sea State CCI {{ cci_version }} 
dataset includes two main components: the first component is the SWH cross-mission 
intercalibration, which aims at reducing the inter-mission bias; the second 
component is the SWH absolute calibration, which aims at reducing the bias between
altimeter missions and in situ SWH measurement from a global wave buoy network. 
An additional component – called intra-calibration - is required for specific 
missions that suffer from temporal heterogeneity. The overall methodology is based
on three types of dataset: 
-  collocated SWH records from two *reference altimetry missions* flying closely 
   behind each other along the same orbit during their tandem phases (hereafter called **tandem records**); 
-  collocated measurements between a reference mission and a non-reference mission 
   at their crossover locations (**crossover records**); 
-  collocated measurements between reference missions and in situ buoy SWH measurements 
   (**buoy matchup records**).

```{admonition} Reference missions
Reference altimetry missions are satellite missions that provide high-accuracy, 
continuous measurements of sea surface height from a stable reference orbit, forming 
the backbone of global sea level monitoring. These missions include carefully coordinated 
tandem phases, during which a new satellite flies closely behind its predecessor 
to enable cross-calibration and ensure data continuity across successive missions. 
In this document reference altimetry missions refers to the current altimetry mission 
Sentinel-6 A (Michael Freilich) and the four historical altimetry missions Topex, 
Jason-1, Jason-2, and Jason-3. Note that for Topex, only data from the Topex Side B 
instrument are used as reference, since the Topex Side A instrument showed signs of 
degradation before the tandem phase with the Jason-1 mission.
```

The bias correction methodology is made of the following steps:
1. the five reference altimetry missions Topex, Jason-1/2/3 and Sentinel-6 A are 
   intercalibrated using tandem SWH records. For this purpose, Jason-3 SWH records 
   are (arbitrarily) considered as the reference and the four other missions are 
   aligned to its records; 
2. the five intercalibrated reference missions are then calibrated against in situ 
   measurements (absolute calibration) using buoy matchups. For this purpose, it 
   is assumed that all reference altimetry missions, once intercalibrated with each 
   other, present similar bias patterns with respect to in situ data so that a single 
   bias correction function can be derived for all missions. This assumption allows 
   to increase the number of altimeter-buoy data pairs and extend the validity range
   of the calibration;
3. the non-reference missions are finally intercalibrated against the bias corrected 
   reference altimetry missions based on the crossover records with the reference 
   mission presenting the longest overlap;

The reference dataset used to correct each altimetry mission of the Sea State CCI 
{{ cci_version }} dataset, as well as the number of available records are presented in 
{numref}`reference_datasets`.

In addition to these three steps, an additional intra-calibration step was necessary 
for three specific missions (Topex Side A, GFO and SARAL), which presented degraded 
performance over a limited time period. This intra-calibration step is further 
described in {numref}`__intercalibration_topex_gfo_saral`.

```{table} Reference dataset and number of records used for the altimeter SWH bias-correction in the Sea State CCI {{ cci_version }} dataset
:name: reference_datasets

| Mission | Reference dataset | Data type | Number of records |
| ------- | ----------------- | --------- | ----------------- |
| **Inter-calibration of reference missions** |
| Sentinel-6 A | Jason-3 | Tandem | 10,032,474 |
| Jason-3 | N/A | N/A | N/A |
| Jason-2 | Jason-3 | Tandem | 10,654,786| 
| Jason-1 | Jason-2 (intercalibrated)| Tandem | 8,703,256 |
| TOPEX Side B | Jason-1 (intercalibrated) | Tandem | 9,364,549 |
| **Absolute calibration of reference missions** |
| TP, J1/2/3,S6 (intercalibrated) | Wave buoys | Matchup | 44,507 |
| **Inter-calibration of non-reference missions** |
| SWOT | Sentinel-6 A (bias-corrected) | Crossover | 10,249 |
| CFOSAT | Jason-3 (bias-corrected) | Crossover | 20,977 |
| Sentinel-3 B | Jason-3 (bias-corrected) | Crossover | 23,710 |
| Sentinel-3 A | Jason-3 (bias-corrected) | Crossover | 32,148 |
| SARAL | Jason-3 (bias-corrected) | Crossover | 11,445 | 
| Cryosat-2 | Jason-2 (bias-corrected) | Crossover | 26,072 |
| Envisat | Jason-1 (bias-corrected) | Crossover | 34,500 |
| GFO | Jason-1 (bias-corrected) | Crossover | 19,181 |
| ERS-2 | Topex Side B (bias-corrected) | Crossover | 11,690 |
| TOPEX Side A | ERS-2 (bias-corrected) | Crossover | 11,364 |
| Poseidon | ERS-2 (bias-corrected) | Crossover | 1,802 |
| ERS-1 | TOPEX Side A (bias-corrected) | Crossover | 11,251 |
```

### Description of the correction method
Here, the correction method refers to any of the tandem-based, crossover-based or 
matchup-based corrections, either used for cross-mission intercalibration or absolute 
calibration of the reference missions. Building up on developments carried out during
the first phase of the Sea State CCI project (Dodet et al. 2020), the correction 
method is based on the generation of mission-dependent Look Up Tables (LUTs) that 
can accommodate for SWH-dependent error structures. A notable evolution compared to 
the previous Sea State CCI datasets (version 1.1, version 3 and version 4) is the
use of an Empirical Quantile Mapping (EQM) technique instead of standard binning 
technique to derive the correction LUTs. Empirical quantile mapping (EQM) is a 
nonparametric bias-correction technique that adjusts the uncorrected dataset so 
that its statistical distribution matches that of the reference dataset. EQM proceeds 
as follows:
1. Empirical cumulative distribution functions (CDFs) are computed for the reference 
  dataset (REF) and the uncorrected dataset (UNC) over the common period of measurements
2. For each observation x in the uncorrected dataset, the percentile $p = CDF_{uncorrected}(x)$ 
   is mapped onto the reference distribution using the inverse CDF of the reference 
   dataset. The corrected value is therefore: $x_{corrected} = CDF_{reference}^{−1}(CDF_{uncorrected}(x_{uncorrected}))$

In practice, the EQM-based correction LUT is obtained by interpolating the difference
between $x_{corrected} - x_{uncorrected}$ onto a fix vector ranging from 0.1 to 25m with an increment of 
0.05m ({numref}`swh_correction`). For high SWH values (typically above 10m), the corrections 
present large fluctuations that result from limited sampling and cannot be considered 
as robust estimate of the mission inter-bias. Therefore, a constant value is fixed 
for SWH larger than a defined maximum SWH value. In addition, for the correction 
of the non-reference missions that relied on a reduced number of crossover samples 
(see {numref}`reference_datasets`), the correction was approximated with a linear fit over a fixed SWH 
range (see magenta dots in {numref}`swh_correction`, bottom right panel). The SWH ranges used for 
the linear approximation and the maximum SWH used for extrapolating the correction 
with a constant values are mission dependent and were defined from visual inspection 
of the raw correction data. These parametric values are listed in {numref}`swh_ranges` for each 
altimetry mission.

```{figure} ../images/swh_correction.png
:name: swh_correction

ECDFs of reference and uncorrected dataset (top panels) and Empirical Quantile 
Mapping bias corrections (bottom panels) derived for Topex side B against Jason-1 
SWH data (left panels) and for Saral against Jason-3 SWH data (right panels). 
In the bottom panels, black dots represent the raw corrections and magenta dots 
represent the final LUT corrections after interpolation and thresholding. 
```

Finally, in order to reduce noise and outliers in the altimeter measurements before 
computing the bias-correction LUTs, the CCI quality level information was applied 
(ie. only measurements with swh_quality_level equal to 3 were used) and the denoised SWH 
records were used at all steps of the bias-correction procedure. In addition, only 
altimeter records located at more than 100km from the coast were used.

```{table} SWH ranges and maximum SWH values used for the linear approximation and extrapolation of the bias-correction LUTs
:name: swh_ranges

| Mission | Linear fitting range (m) | Maximum SWH (m) |
| ------- | ------------------------ | --------------- |
| SWOT | [3 6] | 10 |
| Sentinel-6A/MF | N∕A | 12 |
| CFOSAT | [3 6] | 10 |
| Sentinel-3B | [2 6] | 10 |
| Sentinel-3A | [2 6] | 10 |
| Jason-3 | N∕A | 10 |
| SARAL | [4 8] | 10 |
| Cryosat-2 | [4 7] | 10 |
| Jason-2 | N∕A | 10 |
| Envisat | [4 7] | 10 |
| Jason-1 | N∕A | 10 |
| GFO | [4 7] | 10 |
| ERS-2 | [3.5 5.5] | 10 |
| TOPEX Side B | N∕A | 12 |
| TOPEX Side A | [2.5 6] | 10 |
| Poseidon | [2 4] | 10 |
| ERS-1 | [2.5 6] | 10 |
```

### Selection of in situ platforms for absolute calibration
The CMEMS In Situ Thematic Assembly Center (CMEMS INSTAC) is a component of the 
European Marine Copernicus Service and its role is to ensure consistent and reliable
access to a range of in situ data for service production and validation. For this 
purpose, CMEMS INSTAC collects multi-source/multiplatform data, and performs consistent 
quality control before distributing the data in a common format to the CMEMS Marine 
Forecasting Centres (MFC). The data can be found at http://www.marineinsitu.eu/. 
For the absolute calibration of the SWH records from the reference missions, we have 
considered all altimeter-in situ matchups for which the the altimeter records was 
located at a minimum distance of 50 km from the shore, a maximum distance of 50 from 
the buoy, and  with a maximum time difference of 30min with the buoy records. The 
in situ SWH observations were smoothed with a 1-h running average, in order to reduce 
high-frequency variability. 

In order to ensure that only good quality measurements are selected, a number of 
tests have been applied:
- stationarity: rejecting SWH buoy measurements having a constant SWH value over more than 48 hr;
- location: rejecting SWH buoy measurements over unrealistic position change within a short period of time;
- CMEMS quality check: using the CMEMS native quality flags on time, location and SWH to reject bad or suspect SWH buoy measurements;
- precision: detecting and rejecting SWH buoy measurements having a precision equal or larger than 0.5 meters;
- range: rejecting SWH buoy measurements out of the 0-30 meter range;

(__intercalibration_topex_gfo_saral)=
### Intracalibration of Topex Side A, GFO and SARAL

#### Drift correction of TOPEX Side A SWH 
The TOPEX altimeter was the primary sensor for the TOPEX/POSEIDON mission, launched 
on August 10 1992. It is a redundant instrument, with the two sides designated Side A 
and Side B. The TOPEX Side A altimeter was activated soon after launch. It began to 
show signs of change/degradation in early 1997. In particular, changes in the Side A
point target response were observed as an apparent drift in the measured significant
wave heights and instrument range RMS, and as a result Side A was turned off on 
February 10, 1999 and Side B turned on, becoming the new primary operational altimeter 
(Rosmorduc et al., 2023). The TOPEX Side A and Side B SWH records are therefore 
considered as two separate dataset. In the CCI v5, we use the latest CNES/JPL 
reprocessing: TOPEX/POSEIDON GDR-F Products. As detailed in the product handbook 
(Rosmorduc et al. 2023), the TOPEX altimeter data processing used for this product 
is based upon a numerical retracking approach, which introduces the instrument Point 
Target Response (PTR) to obtain an echo model that better reflects the characteristics 
of the altimeter. Unlike the classical analytical MLE solution, no instrument corrections 
are needed with the numerical retracking approach for range, SWH and sigma0 as a function 
of SWH and off nadir angle (i.e., Look Up Tables are not required or applied). In addition, 
the use of the measured PTR compensates for the impact of the altimeter evolution on 
the range, SWH and sigma0 measurements without having to introduce external corrections. 
In particular, this approach allows for much more stable SWH measurements when 
compared to the original TOPEX dataset. 

Given the objective of long-term cross mission consistency of the CCI v5, intercalibration 
was deemed necessary and a bias-correction LUT was also derived for this mission. 
Since TOPEX Side A was turned off before the launch of the subsequent reference mission (Jason-1), 
its SWH intercalibration is based on crossover records with ERS-2 (see Table 1). 
Moreover, time consistency of TOPEX Side A was assessed through comparisons with the 
Ifremer WW3 wave model hindcast and ECMWF ERA-5 reanalysis, which revealed a slightly 
decreasing trend of the SWH records over the period July 1994 to February 1999. 
This trend was linearly corrected using model results interpolated along the track.

#### SARAL ALtiKa star-tracker anomaly (February 2019 onwards)
The Indo-French (ISRO/CNES) SARAL/AltiKa mission, launched on February 25 2013, 
carries the first spaceborne Ka-band radar altimeter, designed to measure sea-surface 
height, waves, inland water levels, and ice-sheet elevation with higher spatial 
resolution and lower noise than earlier Ku-band missions. In February 2019, one 
of the spacecraft’s star trackers (star sensors used for attitude determination) 
suffered a thermal-related anomaly and stopped tracking correctly, causing degraded
pointing accuracy and increased off-nadir mispointing. This reduced the amount of 
valid altimeter observations and introduced quality issues in sea-surface height and 
wave measurements (Sharma et al., 2019). In order to ensure long-time cross-altimeter 
consistency, the bias-correction LUTs were computed separately over the period 
25/02/2013-31/01/2019 and 01/02/2019-31/12/2025.

#### GFO attitude changes on February 2002
The Geosat Follow-On (GFO) mission was a U.S. Navy radar altimetry satellite launched 
in 1998 to continue the ocean surface topography measurements begun by the earlier 
Geosat mission (not included in Sea State CCI dataset). Comparisons of the long-term
error metrics of GFO SWH records against Ifremer WW3 wave model hindcast and ECMWF 
ERA-5 reanalysis revealed a systematic deviation of the monthly metrics over the 
two first years of the mission operational phase (starting in November 2002). 
While there is no documentation of a degradation of SWH measurements over this period, 
we note from the  GFO Altimeter Engineering Assessment Report (https://ntrs.nasa.gov/api/citations/20040079392/downloads/20040079392.pdf) 
that a much higher than usual numbers of attitudes (above 0.3 degrees) was identified 
over this period, involving a spacecraft attitude change on February 26 2002 (mid-cycle 26). 
Therefore, the bias-correction LUTs were computed separately before the attitude 
change of February 2002, which resulted in improved consistency of the SWH 
time-series.


```{admonition} References
Dodet, G., Piolle, J.-F., Quilfen, Y., Abdalla, S., Accensi, M., Ardhuin, F., Ash, E., Bidlot, J.-R., Gommenginger, C., Marechal, G., Passaro, M., Quartly, G., Stopa, J., Timmermans, B., Young, I., Cipollini, P., Donlon, C., 2020. The Sea State CCI dataset v1: towards a sea state climate data record based on satellite observations. Earth System Science Data 12, 1929–1951. https://doi.org/10.5194/essd-12-1929-2020

Queffeulou, P., 2016. Validation of Jason-3 altimeter wave height measurements. Presented at the OSTST.

Rosmorduc, V., Roinard, H., Desai, S., Desjonqueres, J.-D., Callahan, P.S., Bignalet-Cazalet, F., 2023. TOPEX/POSEIDON GDR-F Products Handbook (No. SALP-MU-MAO-OP-17607-CN).

Sepulveda, H., Queffeulou, P., Ardhuin, F., 2015. Assessment of SARAL/AltiKa Wave Height Measurements Relative to Buoy, Jason-2, and Cryosat-2 Data. Marine Geodesy 38, 449–465. https://doi.org/10.1080/01490419.2014.1000470

Sharma, R., Chaudhary, A., Seemanth, M., Bhowmick, S.A., Agarwal, N., Verron, J., Bonnefond, P., Gupta, H., Thomas, J.V., 2022. SARAL/AltiKa data analysis for oceanographic research: Impact of drifting and post star sensor anomaly phases. Advances in Space Research 69, 2349–2361. https://doi.org/10.1016/j.asr.2021.12.008
```

(__denoising)=
## SWH Denoising

A non-parametric denoising method based on Empirical Mode Decomposition (EMD,
Huang et al., 1998) and inspired by wavelet thresholding is applied to the 
bias corrected variable `swh_adjusted` and stored in `swh_denoised` (see 
Kopsinis and McLaughlin, 2009, Quilfen et al., 2018 and Quilfen and Chapron, 
2019ab).

A detailed description of the method can be found in Quilfen and Chapron, 2019b.

For a given noisy, input signal, the SNR and robustness of the denoised signal 
(e.g. to mitigate for result uncertainties associated with signal fluctuations 
close to the applied thresholds) are increased by estimating the final result 
as an ensemble average of several denoised signals. For that, the noise n1(t) 
is first removed from the noisy signal x(t), then a set of k new noisy signals 
is generated by adding random realisations of n1(t), providing after denoising 
a set of k denoised signals whose average gives the resulting denoised SWH 
`swh_denoised` and whose standard deviation gives the uncertainty attached to 
the denoised SWH (`swh_emd_uncertainty`). The uncertainty parameter 
`swh_emd_uncertainty` therefore accounts for the noise characteristics of the 
noisy signal (function of the altimeter sensor, SWH etc), as well as for the 
local SNR (which is scale-dependent) and for uncertainties attached to the 
denoising process.


```{admonition} References
:class: note

Huang, N.E., Shen, Z., Long, S.R., Wu, M.C., Shih, H.H., Zheng, Q., Yen, N.-C., Tung, C.C.,
Liu, H.H., 1998. The empirical mode decomposition and the Hilbert spectrum for nonlinear
and non-stationary time series analysis. Proceedings of the Royal Society of London A:
Mathematical, Physical and Engineering Sciences 454, 903–995.
https://doi.org/10.1098/rspa.1998.0193

Kopsinis, Y., McLaughlin, S., 2009. Development of EMD-Based Denoising Methods Inspired
by Wavelet Thresholding. IEEE Transactions on Signal Processing 57, 1351–1362.
https://doi.org/10.1109/TSP.2009.2013885

Quilfen, Y., Yurovskaya, M., Chapron, B., Ardhuin, F., 2018. Storm waves focusing and
steepening in the Agulhas current: Satellite observations and modeling. Remote Sensing Of
Environment 216, 561–571. https://doi.org/10.1016/j.rse.2018.07.020

Quilfen, Y., and Chapron, B., 2019. Ocean Surface Wave-Current Signatures From Satellite
Altimeter Measurements. Geophysical Research Letters 46, 253–261.
https://doi.org/10.1029/2018GL081029

Quilfen Y., Chapron B. (2020). On denoising satellite altimeter
measurements for high-resolution geophysical signal analysis.
Advances in Space Research, 68. https://doi.org/10.1016/j.asr.2020.01.005]
```



## SWH Uncertainties

The L2P files contain two different and complementary estimates of 
SWH uncertainty:
- `swh_emd_uncertainty`: this is based on the “noise” estimated from the 
  along-track variability of the EMD denoising applied on the 1 Hz data. It 
  is an estimate of the uncertainty of the debiased and denoised significant 
  wave height.
- `swh_uncertainty`: this is a theoretical estimate of the uncertainty 
  caused by speckle noise and sampling in 1-Hz averaged SWH values. Note 
  that SWH sampling errors at 1Hz can be correlated (e.g. De Carlo et al. 
  2023), so that the average over n 1~Hz values can have an uncertainty 
  larger than 1/sqrt(n) times the 1 Hz uncertainty. 

For the largest wave heights (Hs > 15 m) we advise to make a distinction 
between Hs and SWH: SWH is an average of local wave heights, and it contains 
fluctuations due to sampling uncertainty (i.e. wave groups), and the true 
significant wave height Hs can only be estimated by averaging or denoising. 
We find that the uncertainty on Hs estimated from a 7-point running average 
and applying the theoretical uncertainty model for 7 s averages is close to 
the EMD denoising uncertainty estimate. 

The theoretical uncertainty is fully described in De Carlo and Ardhuin (2024), 
and the variance is the sum of a variance caused by speckle noise and 
sampling (wave group effect).

For speckle noise, it is a function of the wave height Hs, the number of 
pulses averaged $n_a$, and the retracking method: Maximum Likelihood (ML), 
WHALES or Least Squares (LS), and a variance caused by sampling (wave group 
effect) which is a function of the wave height Hs, the satellite altitude h 
and the spectral shape quantified by the peakedness $Q_{kk}$. 

The processing algorithm sets the value of s0 which gives the effect of 
speckle noise:  it is lowest for ML (s0=1 m), intermediate for WHALES (s0 ~ 2 m) and largest for LS (s0=5 m). 

{numref}`sat_uncertainties` shows estimated of (a) the effective spatial resolution of 
the along-track altimeter data: this is roughly the Chelton et al. (1989) 
radius $\rho_c=sqrt(2 h Hs)$ divided by 1.5, this is smallest for CFOSAT 
because of the much lower orbit (b) the uncertainty of the data at the 
native rate (4.5 Hz for CFOSAT, 40 Hz for SARAL and 20 Hz for all others) 
and (c) the uncertainty of SWH averaged over 1 Hz. Note that there was a 
mistake in a similar figure of De Carlo and Ardhuin (2024) for CFOSAT (the 
native data rate was not properly taken to be 4.5 Hz) 

```{figure} ../images/all_sat_uncertainties.png
:name: sat_uncertainties
```

Different spectral shapes are considered: $Q_{kk}$ = 2 Hs is typical of a wind 
sea, whereas swell-dominated conditions often have $Q_{kk}$ > 60 m. 


(__sigma0_bias_correction)=
## Sigma0 bias correction

### Background
The normalised radar cross-section, $σ^0$, is  a measure of the reflectance of the 
Earth surface at nadir, which is calculated as the strength of the received signal 
divided by the emitted pulse, with allowance for known losses. As the backscatter 
is usually expressed in logarithmic terms (decibels), nearly all physical causes 
of loss (degradation of amplifier, loss in reflectivity of antenna etc.) equate 
to a simple subtraction in dB.  The strength of the emitted signal is monitored 
and compensated for in the on-ground processing.  There are two concerns: the 
difference in $σ^0$ definition/calculation for individual missions, and changes 
in $σ^0$ performance that are not correctly picked up by the internal monitoring. 
They are both addressed in the same way.

### Constant reference surfaces
Use of surfaces with constant reflective or emissive properties is a common method 
of monitoring of satellite sensors, but for nadir-pointing radar altimeter there 
is no natural surface that provides the required accuracy.  However, the relationship 
between ocean scattering at Ku- and C-band is very well constrained and was thus 
proposed as a method of monitoring $\sigma^0$ variations (Quartly, 2000).  There are minor 
secondary effects caused by changes in  wave height and sea surface temperature, 
but these are fully documented (Quartly, 2025). To intercompare different instruments 
requires a consistent definition of $\sigma^0$, in effect use of the same retracker 
throughout.  Earlier work showed the MLE-3 retracker to be very robust, but this 
is not available for some recent missions, so instead we use the "adjusted $\sigma^0$", 
which is the MLE-4 estimate compensated for waveform-derived mispointing, 
$\psi^2$ (Quartly, 2009). Although the $\sigma^0$ at C-band is usually based on an MLE-3 
retracker, it often utilises the $\psi^2$ estimate from Ku-band.

### Observed sigma0 differences
{numref}`sigma0_correction_1` shows plots of the mean relationship between 
$\sigma^0_{Ku}$ and $\sigma^0_{C}$ for a number 
of dual-frequency radar altimeters that successively occupied the Topex/Jason 
reference orbit   Simple shifts (of order a few dB) align these different empirical 
curves closely, providing a consistent sigma0 record.  During lifetimes of individual 
missions there may be minor changes in calibration (usually less than 0.1 dB) that 
are required to maintain the match to the defined reference curve.  For Jason-2 
alone, there were also noted to be issues with the on-ground AGC corrections, so 
a further correction term is needed specifically for that instrument.

```{figure} ../images/SOMA_s0s0_unshifted.png
:name: sigma0_correction_1

Mean $σ^0$-$σ^0$ relationships for several dual-frequency altimeters, showing the 
large offsets required to bring them into alignment.
```

```{figure} ../images/SOMA_sig0_corr_Topex.png
:name: sigma0_correction_2

Time series of shifts for the Topex altimeters (switch from Topex-A to Topex-B 
was in Feb. 1999), showing the large overall value, due to instrument processing 
specification, with much smaller changes added to that. 
[The thin blue (orange) line shows the inferred correction at Ku- (C-) band on 
approximately monthly analysis; the thick yellow (purple) line indicates the 
implemented corrections, which ignore the short-term variations.]
```

### Implementation

- Ku-band: $σ^0_{CCI}=σ^0_{MLE4}-α_{Ku}\psi^2 + \Delta σ^0_{Ku}(t)  - \delta σ^0_{Ku}(AGC_{Ku})$
- C-band: $σ^0_{CCI}=σ^0-α_C \psi^2 + \Delta σ^0_{C}(t)  - \delta σ^0_{C}(AGC_{C})$

where the coefficients, $α_{Ku}$ and $α_{C}$, are specific for each altimeter mission, and 
the AGC term is only needed for Jason-2.  The corrections varying with $\psi^2$ and AGC 
effect changes in $σ^0$ on small spatial scales leading to better short-term consistency; 
the $\Delta σ^0$ term is designed to apply the large shifts needed between different missions 
and to compensate for uncorrected long-term drift.

These corrections were only derived on a selection of missions, for a given version 
of the input GDR (which the $σ^0$ are taken from), as listed in {numref}`sigma0_corrected_missions`:

```{table} GDR version and used 1 Hz $\sigma^0$ variables for which a bias corrected \sigma^0$ is provided.
:name: sigma0_corrected_missions

| source                   | period             | GDR version | $σ^0_{Ku}$ variable | $σ^0_{C}$ variable |
|--------------------------|--------------------|-------------| ------------------- | ------------------ |
| Jason-1       | 01/2002 to 07/2013 | Version E   | `sig0_ku` | `sig0_c` |
| Jason-2       | 06/2008 to 10/2019 | Version D   | `sig0_ku` | `sig0_c` |
| Jason-3       | 02/2016 to 01/2025 | Version F   |  `ku_sig0_ocean` | `c_sig0_ocean` |
| Topex         | 08/1992 to 01/2006 | Version F   | `sig0_ku` | `sig0_c` |
| Sentinel-6 A  | 03/2020 to 12/2025 | Version G   |   `ku_sig0_ocean` | `ku_sig0_ocean` |
| Sentinel-3 A  | 02/2016 to 11/2024 | Version 005    | `sig0_ocean_20_plrm_ku` | `sig0_ocean_20_c` |
| Sentinel-3 B  | 04/2018 to 11/2024 | Version 005    | `sig0_ocean_20_plrm_ku` | `sig0_ocean_20_c` |
```



```{admonition} References
:class: note
Quartly G.D. 2000, Monitoring and cross-calibration of altimeter σ0 through dual-frequency backscatter measurements, J. Atmos. Oceanic Tech., 17, 1252-1258.

Quartly, G.D., 2009, Optimizing σ0 information from the Jason-2 altimeter. IEEE Geosci. Rem. Sensing Lett., 6 (3), 398-402. doi: 10.1109/LGRS.2009.2013973

Quartly, G.D., 2025. The intertwined factors affecting altimeter sigma0. Remote Sens. 17, 3776 (21pp.). doi: 10.3390/rs17223776
```

(__wind_speed_calculation)=
## Wind speed calculation

The wind speed measured by the altimeter is calculated from the $\sigma^0$ 
measurements, using the Abdalla (2012) formulation. Other algorithms using 
additional dependencies such the SWH and sea surface temperature are being 
assessed with CCI Sea State project team and may be added in future.

```{admonition} References
:class: note
S. Abdalla (2012) Ku-Band Radar Altimeter Surface Wind Speed Algorithm, Marine Geodesy, 35:sup1, 276-298, DOI: 10.1080/01490419.2012.718676"
```

