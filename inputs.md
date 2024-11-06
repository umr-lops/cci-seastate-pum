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
| Mission     | Path                                                                                                                                                                                                             |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CRYOSAT-2   | /home/datawork-cersat-public/provider/esa/satellite/l1b/cryosat-2/siral/sir_lrm_1b/version_d/data/date & /home/datawork-cersat-public/provider/esa/satellite/l1b/cryosat-2/siral/sir_lrm_1b/version_e/data/date/ |
| SARAL       | /home/datawork-cersat-public/provider/aviso/satellite/l2/saral/altika/sgdr/version_f/data/date                                                                                                                   |
| JASON-1     | /home/datawork-cersat-public/provider/aviso/satellite/l2/jason-1/poseidon-2/sgdr/version_e/data/date                                                                                                             |
| JASON-2     | /home/datawork-cersat-public/provider/aviso/satellite/l2/jason-2/poseidon-3/sgdr/version_d/data/date/                                                                                                            |
| JASON-3     | /home/datawork-cersat-public/provider/aviso/satellite/l2/jason-3/poseidon-3b/sgdr/version_f/data/date                                                                                                            |
| TOPEX       | /home/datawork-cersat-public/provider/aviso/satellite/l2/topex-poseidon/topex/gdr/version_f/                                                                                                                     | 
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