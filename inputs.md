# Input data

## Altimetry

This sections lists the input waveform data used for the CCI Sea State altimeter 
datasets. Note that only some of these missions were retracked within the CCI 
Sea State project, while for others the 1 Hz measurements were produced directly 
from the 20 Hz measurements available from the provider's retracker. Refer to 
{numref}`swh_retrackers` to check the retracker associated with each input in 
this table.


```{table} List of SGDR input data
:name: sgdr_inputs
| Mission              | Provider  | Path                                                                                                    | Date                          |
|----------------------|-----------|---------------------------------------------------------------------------------------------------------|-------------------------------|
| CRYOSAT-2 Version E  | ESA       | /home/datawork-cersat-public/provider/esa/satellite/l1b/cryosat-2/siral/sir_lrm_1b/version_e/data/date/ | From 16/07/2010 to 31/12/2023 |
| SARAL Version F      | AVISO     | /home/datawork-cersat-public/provider/aviso/satellite/l2/saral/altika/sgdr/version_f/data/date          | From 14/03/2013 to 31/12/2023 |
| JASON-1 Version E    | AVISO     | /home/datawork-cersat-public/provider/aviso/satellite/l2/jason-1/poseidon-2/sgdr/version_e/data/date    | From 15/01/2002 to 21/06/2013 |
| JASON-2 Version D    | AVISO     | /home/datawork-cersat-public/provider/aviso/satellite/l2/jason-2/poseidon-3/sgdr/version_d/data/date/   | From 04/07/2008 to 01/10/2019 |
| JASON-3 Version D    | AVISO     | /home/datawork-cersat-public/provider/aviso/satellite/l2/jason-3/poseidon-3b/sgdr/version_d/data/date   | From 17/02/2016 to 06/05/2019 |
| JASON-3 Version F    | AVISO     | /home/datawork-cersat-public/provider/aviso/satellite/l2/jason-3/poseidon-3b/sgdr/version_f/data/date   | From 07/05/2019 to 31/12/2023 |
| TOPEX Version F      | AVISO     | /home/datawork-cersat-public/provider/aviso/satellite/l2/topex-poseidon/topex/gdr/version_f/            | From 13/10/1992 to 04/10/2005 |
| ERS-1                | ESA       | /home/datawork-cersat-public/provider/esa/satellite/l2/ers-1/ra/esa-reaper/ers_alt_2_/data/date/        | From 03/08/1991 to 02/06/1996 |
| ERS-2                | ESA       | /home/datawork-cersat-public/provider/esa/satellite/l2/ers-2/ra/esa-reaper/ers_alt_2_/data/date/        | From 14/05/1995 to 02/07/2003 |
| ENVISAT Version 3    | ESA       | /home/datawork-cersat-public/provider/esa/satellite/l2/ers-2/ra/esa-reaper/ers_alt_2_/data/date/        | From 14/05/2002 to 08/04/2012 |
| Sentinel-3A Version 005 | EUMETSAT | /home/datawork-cersat-public/provider/eumetsat/satellite/l2/sentinel-3a/sral/BC005_Release/      | From 01/07/2016 to 31/12/2023 |
| Sentinel-3B Version 005 | EUMETSAT | /home/datawork-cersat-public/provider/eumetsat/satellite/l2/sentinel-3b/sral/BC005_Release/      | From 08/05/2018 to 31/12/2023 |
| Sentinel-6A Version F08 | EUMETSAT | /home/datawork-cersat-public/provider/eumetsat/satellite/l2/sentinel-6a/poseidon-4/p4_2__lr/f08/ | From 17/12/2020 to 31/12/2023 |
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

```{admonition} Note on Jason-3
Two versions of SGDR were used as input : **version D** and **Version F**. 
Version F replaced version D in the course of the mission. However a large 
amount of data in version D had already retracked with WHALES in CCI Sea State. 
It was decided not to replace this already completed segment of the Jason-3 
data archive.

Data processed in version D and version F were however separated in CCI 
processing to ensure that possible inconsistencies between the two versions are 
addressed.

Jason-3 SGDR data during the cal/val period are labelled as **version T**.
Version T method and algorithms are consistent with version D. The two versions 
are merged into the version D segment of the CCI processing.
```

## SAR

This sections lists the input Level 1 data used for the CCI Sea State SAR 
datasets.

```{table} List of SAR L1B input data
:name: sar_l1b_inputs
| Mission                 | Provider | Path                                                                                             | Date                          |
|-------------------------|----------|--------------------------------------------------------------------------------------------------|-------------------------------|
```
