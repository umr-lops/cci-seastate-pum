# Nadir-altimetry processing details

This section describes the processing baseline for the altimeter data in 
CCI Sea State {{cci_version}}.

(__retracking)=
## Retracking
Some missions were retracked specifically for the CCI Sea State dataset, 
using WHALES retracker, whereas in some cases the data retracked by other 
agencies were used instead. 

{numref}`swh_retrackers` summarized the retracker used for each 
mission for significant wave height, whether the retracking was performed by 
CCI Sea State production team (WHALES) or a third party agency:

```{table} Retracker used for each mission for retrieving the significant wave height
:name: swh_retrackers

| source                   | period             | retracker     | 
|--------------------------|--------------------|---------------|
| ERS-1                    | 07/1991 to 03/2000 | REAPER (MLE3) | 
| ERS-2                    | 04/1995 to 07/2011 | REAPER (MLE3) | 
| Jason-1 Version E        | 01/2002 to 07/2013 | WHALES        | 
| Jason-2 Version D        | 06/2008 to 10/2019 | WHALES        | 
| Jason-3 Version D        | 09/2016 to 06/2019 | WHALES        | 
| Jason-3 Version F        | 06/2019 to now     | WHALES        | 
| Jason-3 Version T        | 02/2016 to 09/2016 | WHALES        | 
| Topex Version F          | 08/1992 to 01/2006 | MLE3          |
| Envisat Version 3        | 03/2002 to 04/2012 | WHALES        | 
| CryoSat-2  Version E     | 07/2010 to now     | WHALES        | 
| SARAL Version F          | 02/2013 to now     | WHALES        |
| Sentinel-6 A Version F08 | 03/2020 to 12/2023 | MLE4 (?)      |   
| Sentinel-3 A Version 005 | 02/2016 to now     |               |
| Sentinel-3 B Version 005 | 04/2018 to now     |               |
```

(__whales)=
### WHALES retracker

(__compression)=
## Compression to 1 Hz

This section summarizes how the full resolution (20/40 Hz) measurements are 
edited and compressed into 1 Hz measurements. The same full resolution to 1 Hz 
measurement mapping is used as in agency (S)GDR products so that a CCI Sea 
State 1 Hz file is fully comparable to the corresponding GDR file. 

### Land detection
- full resolution (20/40 Hz) SWH and sigma0 values are flagged as land when 
  their distance to coast is **greater than 1000m**, based on the Goddard Space 
  Flight Center 1km resolution grid of distance to coast. They are ignored 
  in the compression process.

### Significant wave height (SWH)

1. full resolution measurements flagged as bad by the retracker are discarded. 
   20 Hz 
measurements are flagged as bad, for altimeters retracked with WHALES, when the
fitting error is greater than 0.3.
2. 20 Hz measurements located on land are discarded. The land location is determined
from the latitude & longitude provided for each measurement using OpenstreetMap
coastline, with a precision of about 50m.
3. 20 Hz SWH measurements not in the -0.5 to 30 meter range are discarded
4. the remaining 20 Hz SWH outliers are discarded. The outlier detection scheme is
based on the maximum absolute deviation for a 3-sigma criterion, meaning
○ only 20 Hz SWH values within [median(SWH) - 3 * MAD(SWH),
median(SWH) 3 * MAD(SWH)] interval are kept
○ with: MAD(SWH) = 1.4286 * median(abs(SWH - median(SWH)))

- SWH for uncompressed (20/40 Hz) measurements are discarded if not in the 
  range: [-0.5, 30]
- when available, the full resolution quality flag is also used to discard 
  invalid full resolution measurements, as summarized in {numref}`fullres_swh`

```{table} selected variable for SWH in each full resolution dataset, and the corresponding quality flag variable used to discard invalid measurements
:name: fullres_swh

| Source                  | SWH                    | SWH quality                         |
|-------------------------|------------------------|-------------------------------------|
| Jason-1 Version E       | swh.07                 | swh_WHALES_fitting_error_20hz > 0.3 |
| Jason-2 Version D       | swh.07                 | swh_WHALES_fitting_error_20hz > 0.3 |
| Jason-3 Version D       | swh_WHALES_20hz        | swh_WHALES_fitting_error_20hz > 0.3 |
| Jason-3 Version F       | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| Jason-3 Version T       | swh_WHALES_20hz        | swh_WHALES_fitting_error_20hz > 0.3 |
| Topex Version F         | swh_20hz_ku            | swh_used_20hz_ku == 0               |
| Envisat Version 3       | swh_WHALES_20hz        | swh_WHALES_qual_20hz == 0           |
| ERS-1 REAPER            | swh_20hz               | swh_used_20hz == 0                  |
| ERS-2 REAPER            | swh_20hz               | swh_used_20hz == 0                  |
| CryoSat-2 Version E     | swh_WHALES_20hz        | swh_WHALES_fitting_error_20hz > 0.3 |
| Saral Version F         | swh_WHALES_20hz        | swh_WHALES_fitting_error_20hz > 0.3 |                           
| Sentinel-6A Version F08 | swh_ocean              | swh_ocean_qual == 1                 |
| Sentinel-3A Version 005 | swh_ocean_20_plrm_ku   | swh_ocean_qual_20_plrm_ku == 0      |
| Sentinel-3B Version 005 | swh_ocean_20_plrm_ku   | swh_ocean_qual_20_plrm_ku == 0      |
```

- uncompressed (20/40 Hz) measurement outliers are discarded using the maximum
  absolute deviation for 3-sigma criterion (MAD) scheme
- the median of the remaining measurements is selected as 1 Hz value 
- when there were less than 6 (12 for SARAL) valid remaining measurements, the 
  1 Hz value is flagged as **bad** in the `quality_level` variable. 
- for some retrackers, a correction is applied to the uncompressed (20/40 Hz) 
  measurements, into a `swh_corrected` variable. The same scheme is applied to
  these corrected measurements to provide a 1 Hz corrected SWH value. The 
  corrected missions are listed in {numref}`fullres_swh_correction`. The 
  corrections were estimated by G.Quartly for version 2 CCI dataset (provided
  on 29-Apr-2021)
  
```{table} missions for which a correction is applied to full resolution SWH measurements
:name: fullres_swh_correction

| Source                  | Correction of SWH      |
|-------------------------|------------------------|
| Jason-1 Version E       | Yes                 |
| Jason-2 Version D       | Yes                 |
| Jason-3 Version D       | Yes        |
| Jason-3 Version F       | Yes        |
| Jason-3 Version T       | Yes        |
| Topex Version F         |             |
| Envisat Version 3       | Yes        |
| ERS-1 REAPER            |                |
| ERS-2 REAPER            |                |
| CryoSat-2 Version E     | Yes                       |
| Saral Version F         | Yes                       |
| Sentinel-6A Version F08 |               |
| Sentinel-3A Version 005 |  |
| Sentinel-3B Version 005 |    |
```

### Averaging of the 20 Hz measurements

The CCI Sea State Dataset {{cci_version}} provides 1 Hz SWH measurements. 
These 1 Hz measurements are calculated by averaging groups of consecutive 
full resolution 20 Hz (18 Hz for Topex, 40 Hz for SARAL). 

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


### Sigma0
- sigma0 are **always taken from third party (agency) SGDR**, in C and Ku band 
  (Ka for SARAL) - using MLE3 Ku band sigma0 when available, as summarized in 
  {numref}`fullres_sigma0`
- when available, the full resolution quality flag is also used to discard 
  invalid full resolution measurements, as summarized in {numref}`fullres_sigma0`

```{table} selected variable for sigma0 in each full resolution dataset, for each sensing band, and the corresponding quality flag variable used to discard invalid measurements
:name: fullres_sigma0
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
| Envisat Version 3        | sig0_ocean_20_ku      | sig0_ocean_qual_20_ku == 0               |
| ERS-1 REAPER             | ocean_sig0_20hz       | ocean_sig0_used_20hz                     |
| ERS-2 REAPER             | ocean_sig0_20hz       | ocean_sig0_used_20hz                     |
| CryoSat-2  Version D     | sig0_1_20_ku          |                                          |
| CryoSat-2  Version E     | sig0_1_20_ku          |                                          |
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

> **S-band sigma0** were discarded for **Envisat** as it was found they were 
> systematically flagged as bad in the SGDR product.


- sigma0 for uncompressed (20/40 Hz) measurements are discarded if not in the 
  range: [7, 30] (except for SARAL Ka sigma0: [-6.5, 23.])
- uncompressed (20/40 Hz) measurement outliers are discarded using the maximum
  absolute deviation for 3-sigma criterion (MAD) scheme
- the median of the remaining measurements is selected as 1 Hz value 
- when there were less than 6 (12 for SARAL) valid remaining measurements, the 
  1 Hz value is flagged as **bad** in the `quality_level` variable.
  

## L2P Processing

The base 1 Hz compressed files are enriched with additional variables and 
consolidated into L2P products.

(__swh_quality_level)=
### SWH quality level

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

#### Test on SWH RMS

A Look-up Table (LUT) is computed for each mission on a set of cycles 
corresponding to periods of nominal orbit and nominal functioning of its  
altimeter. It provides the threshold of the RMS of SWH over the full 
resolution measurements (estimated during the compression to 1 Hz) with 
respect to the estimated 1 Hz SWH : any 1 Hz SWH measurements for which the  
corresponding RMS of the full resolution SWH values averaged to produce this 
1 Hz value are flagged as bad measurements.

The cycles selected to build these LUTs, the methodology and resulting 
average LUTs are described in further details [here](swh_rms_lut).


### Ancillary atmospheric model variables

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

```{table} ERA5/WAM wave model variables added to each 1 Hz measurement in L2P
:name: ancillary_era5wam

| Variable   | Description                                         |
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

### Ancillary sea ice concentration

Different sources are combined for sea ice concentration, as the best 
resolution datasets (25 km) do not cover the full CCI Sea State temporal 
coverage.

```{table} sources for sea ice concentration (SIC) CDR, by order of priority
:name: ancillary_sic

|                      | Variable | Temporal Coverage     | Description                                                                                                                                                 |
|----------------------|----------|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| OSISAF-AMSR-CDR-v3p0 | ice_conc | 2002-2020 (ext: 2024) | AMSR Sea Ice Concentration Climate Data Record from OSI SAF (doi: 10.15770/EUM_SAF_OSI_0015)                                                                |
| SICCI-HR-SIC         | ice_conc | 1991-2020             | High(er) Resolution Sea Ice Concentration Climate Data Record Version 3 from CCI Sea Ice+ (SSM/I and SSMIS) (doi: 10.5285/eade27004395466aaa006135e1b2ad1a) |
```

(__bias_correction)=
## SWH bias correction

The bias correction method implemented for the Sea State CCI version4 
dataset (hereafter CCI v4) includes two main components. The first component 
is the SWH cross-mission intercalibration, which aims at reducing the  
inter-mission bias. The second component is the SWH absolute calibration,  
which aims at reducing the bias between altimeter missions and in situ SWH  
measurement from a global wave buoy network. As implemented in the CCI v4, 
the method is based on three types of dataset: 
- collocated SWH records from two reference missions (see definition at the 
  end of this section) sharing the same orbit during their tandem phases (hereafter called tandem records); 
- collocated measurements between a reference mission and a non-reference mission at their crossover locations (crossover records); 
- collocated measurements between reference missions and in situ buoy SWH measurements (buoy matchup records).

The general organization of the bias correction methodology is made of the 
following steps:
- the five reference missions are intercalibrated using tandem SWH records. 
  For this purpose, Jason-3 SWH records are considered as unbiased and the 
  four other missions are aligned to its records; 
- the five intercalibrated reference missions are calibrated against in situ 
  measurements (absolute calibration) using buoy matchups. For this purpose,
  it is assumed that all reference missions, once intercalibrated with each 
  other, present similar bias patterns with respect to in situ data so that 
  a single bias-correction function can be derived for all missions. This 
  assumption allows to increase the number of altimeter-buoy data pairs and  
  extend the validity range of the calibration ;
- the non-reference missions are intercalibrated against reference missions 
  based on crossover records with the reference mission presenting the 
  longest overlap with them;

### Description of the correction method
Here, the correction method refers to any of the tandem-based, 
crossover-based or matchup-based corrections, either used for cross-mission 
intercalibration or absolute calibration of the reference missions. Building 
up on developments carried out during the first phase of the Sea State CCI 
project (Dodet et al. 2020), the correction method is based on data binning 
technique and results in Look Up Tables (LUT) that can accommodate for 
SWH-dependent error structures. First, the median values of the SWH 
residuals between uncorrected and reference dataset (see Table 1) are 
computed for 0.20m-SWH bin width, with a 0.05 m increment, over the full SWH 
range. When the number of data pairs within a given bin is too low (below 50)
, a Not-a-Number (NaN) value is assigned to this particular bin. This 
systematically occurs at the low and high ends of the SWH distribution. In 
order to avoid abrupt change in the bias-corrected SWH field, the 
corrections are extrapolated at the low end using the first non-NaN value. 
For SWH above 10m, the corrections are extrapolated using the correction at 
SWH=10m. {numref}`swh_correction_1` gives an illustration of the method for the 
intercalibration step between Sentinel-6A and Jason-3 missions during their 
tandem phase.

```{figure} ../images/swh_correction_1.png
:name: swh_correction_1

Median of the differences between SWH (tandem) records from Sentinel-6A/MF and Jason-3 computed over SWH bins of 0.2m and shifted by 0.05m. Grey dots show each data pair differences (22,442,832 records), blue dots show the median value for each bin, ligh and dark shaded region indicate the [25-75%] and [5-95%] limits of the distribution for each bin, and magenta dots show the final Look Up Table after extrapolation of the correction above 10m.
```


However, for most of the non-tandem cases, the number of data pairs is 
generally too low between 5 to 10m so that the correction LUT contains NaN 
values or appear noisy overt this range. In such cases, a robust linear 
regression is fitted through the median of the residuals over a mission-specific 
SWH range (generally between 2 to 7m) to model the error function above a 
specific threshold. {numref}`swh_correction_2` provides such an example with the 
intercalibration between Sentinel-3A and Jason-3. These thresholds and ranges 
are estimated by visual inspection of the data in order to ensure an accurate 
representation of the bias structure and a smooth transition between the 
non-linear corrections for low-to-medium sea state conditions and the linear 
corrections for medium-to-high sea state conditions. Finally, a 5-point 
moving average is applied to the corrections in order to reduce small scale 
oscillations.

```{figure} ../images/swh_correction_2.png
:name: swh_correction_2

Median of the differences between SWH (crossover) records from Sentinel-3A and Jason-3 computed over SWH bins of 0.2m and shifted by 0.05m. Grey dots show each data pair differences (14,224 records), blue dots show the median value for each bin, ligh and dark shaded region indicate the [25-75%] and [5-95%] limits of the distribution for each bin, and magenta dots show the final Look Up Table after extrapolation of the correction above 10m.
```

### Selection of in situ platforms for absolute calibration
The CMEMS In Situ Thematic Assembly Center (CMEMS INSTAC) is a component of the European Marine Copernicus Service and its role is to ensure consistent and reliable access to a range of in situ data for service production and validation. For this purpose, CMEMS INSTAC collects multi-source/multiplatform data, and performs consistent quality control before distributing the data in a common format to the CMEMS Marine Forecasting Centres (MFC). The data can be found at http://www.marineinsitu.eu/. For the absolute calibration of the SWH records from the reference missions, we have considered all altimeter-in situ matchups for which the the altimeter records was located at a minimum distance of 50 km from the shore, a maximum distance of 50 from the buoy, and  with a maximum time difference of 30min with the buoy records.
In order to ensure that only good quality measurements are selected, a number of tests have been applied:
- stationarity: rejecting SWH buoy measurements having a constant SWH value 
  over more than 48 hr;
- location: rejecting SWH buoy measurements over unrealistic position change 
  within a short period of time;
- CMEMS quality check: using the CMEMS native quality flags on time, location 
  and SWH to reject bad or suspect SWH buoy measurements;
- precision: detecting and rejecting SWH buoy measurements having a precision 
  equal or larger than 0.5 meters;
- range: rejecting SWH buoy measurements out of the 0-30 meter range;

- Matchups between altimeter and in situ platform measurements were computed within 100-km distance and 1-h time window. The in situ SWH observations were smoothed with a 1-h running average, in order to reduce high-frequency variability. 

```{note} Note on the drift correction of TOPEX Side A SWH
The TOPEX altimeter was the primary sensor for the TOPEX/POSEIDON mission. It is a redundant instrument, with the two sides designated Side A and Side B. The TOPEX Side A altimeter was activated soon after launch. It began to show signs of change/degradation in early 1997. In particular, changes in the Side A point target response were observed as an apparent drift in the measured significant wave heights and instrument range RMS, and as a result Side A was turned off on February 10, 1999 and Side B turned on, becoming the new primary operational altimeter (Rosmorduc et al., 2023). The TOPEX Side A and Side B SWH records are therefore considered as two separate dataset. In the CCI v4, we use the latest CNES/JPL reprocessing : TOPEX/POSEIDON GDR-F Products. As detailed in the product handbook (Rosmorduc et al. 2023), the TOPEX altimeter data processing used for this product is based upon a numerical retracking approach, which introduces the instrument Point Target Response (PTR) to obtain an echo model that better reflects the characteristics of the altimeter. Unlike the classical analytical MLE solution, no instrument corrections are needed with the numerical retracking approach for range, SWH and sigma0 as a function of SWH and off nadir angle (i.e., Look Up Tables are not required or applied). In addition, the use of the measured PTR compensates for the impact of the altimeter evolution on the range, SWH and sigma0 measurements without having to introduce external corrections. In particular, this approach allows for much more stable SWH measurements when compared to the original TOPEX dataset. Given the objective of long-term cross mission consistency of the CCI v4, intercalibration was deemed necessary and a LUT was also derived for this mission. However, since TOPEX Side A was turned off before the launch of the subsequent reference mission (Jason-1), its SWH intercalibration is based on crossover records with ERS-2. Time consistency of TOPEX Side A was assessed through comparisons with the Ifremer WW3 and ECMWF ERA-5 wave model hindcasts, which revealed a slightly decreasing trend of the SWH records over the period July 1994 to February 1999. This trend was linearly corrected using model results interpolated along the track.
```

Definitions

Reference missions: 
Altimetry reference missions are satellite missions that provide high-accuracy, continuous measurements of sea surface height from a stable reference orbit, forming the backbone of global sea level monitoring. These missions include carefully coordinated tandem phases, during which a new satellite flies closely behind its predecessor to enable cross-calibration and ensure data continuity across successive missions. In this document reference altimetry missions refers to the current altimetry mission Sentinel-6 Michael Freilich and the four historical altimetry missions Topex-Poseidon, Jason-1, Jason-2, and Jason-3.


### SWH data editing
SWH RMS Look Up Table
As documented in Sepulveda et al. (2015), SWH measurements derived from radar altimeter measurements can be contaminated by the presence of land in the footprint, strong rain events, and so-called “sigma0 blooms” due to weak winds or surface slicks. According to these authors, the standard deviation of 20Hz SWH values over one second (hereinafter called SWH RMS) is one of the most relevant parameter to detect erroneous values of SWH. Since the SWH RMS level strongly depends on SWH, constant threshold values are not adequate to efficiently remove SWH RMS outliers.  Therefore, Sepulveda et al. (2015) and Queffeulou (2016) proposed a methodology to set a statistical threshold on SWH RMS that depends on SWH, and which can be used a posteriori to filter out erroneous SWH measurements. Since the SWH RMS for a given narrow SWH presents a log-normal distribution, these authors proposes to estimate the upper threshold for the logarithm of SWH RMS as the sum of the mean value and thrice the standard deviation. Moreover, in order to reduce discontinuity in the SWH RMS threshold function for large SWH values, where the number of records is too low to derive robust statistics, a second-order polynomial function is fitted. 
The methodology implemented to determine a SWH RMS LUT for each mission of the Sea State CCI version 4 dataset can be described as follows: 

1. eight cycles of SWH and SWH RMS measurements are sampled and invalid measurements are rejected based on the land mask and SWH range quality flags; 
2. for each cycle the mean and standard deviation of log(SWH RMS) are computed for SWH bins of 0.5 m width, ranging from 0 to 15 m, with a 0.05 m increment. Only bins with more than 100 values are considered; 
3. upper threshold on SWH RMS are computed for each SWH bin as :
4. exp(mean(log(SWH RMS))+3*std(log(SWH_RMS)));
5. a second-order polynomial function is fitted to the SWH RMS threshold function for SWH values comprised between 3 and 10m and is extrapolated up to SWH = 12m;
6. a look-up table (LUT) of SWH RMS thresholds is derived by combining for each cycle the SWH RMS threshold values for SWH lower than 3m and the values of the polynomial function for SWH higher than 3m; for SWH above 12 m, a constant value equal to the SWH RMS threshold at 12m is taken;
7. the final LUT is computed as the ensemble mean of the 8 cycle LUTs;

Note that the selection of the lower and upper bounds (3-10m) used to estimate the second-order polynomial function are mission specific, and is determined for each method from visual inspection of the SWH_RMS = f(SWH) function.


## References:

Dodet, G., Piolle, J.-F., Quilfen, Y., Abdalla, S., Accensi, M., Ardhuin, F., Ash, E., Bidlot, J.-R., Gommenginger, C., Marechal, G., Passaro, M., Quartly, G., Stopa, J., Timmermans, B., Young, I., Cipollini, P., Donlon, C., 2020. The Sea State CCI dataset v1: towards a sea state climate data record based on satellite observations. Earth System Science Data 12, 1929–1951. https://doi.org/10.5194/essd-12-1929-2020

Queffeulou, P., 2016. Validation of Jason-3 altimeter wave height measurements. Presented at the OSTST.

Rosmorduc, V., Roinard, H., Desai, S., Desjonqueres, J.-D., Callahan, P.S., Bignalet-Cazalet, F., 2023. TOPEX/POSEIDON GDR-F Products Handbook (No. SALP-MU-MAO-OP-17607-CN).

Sepulveda, H., Queffeulou, P., Ardhuin, F., 2015. Assessment of SARAL/AltiKa Wave Height Measurements Relative to Buoy, Jason-2, and Cryosat-2 Data. Marine Geodesy 38, 449–465. https://doi.org/10.1080/01490419.2014.1000470