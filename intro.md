# CCI Sea State Datasets and System Documentation

This document describes the different datasets produced within the **ESA CCI 
Sea State** project, how to install and run the production system and its 
processors, and traces its evolutions and changes.

The current version of CCI Sea State datasets and production system is 
**version 4**.

Three kinds of products are delivered:
* **L2P** : Along-track products from altimetry and SAR (synthetic 
  aperture radar missions  separated per satellite and pass, including all  
  measurements with flags, corrections and extra parameters from other 
  sources. These are expert products with rich content and no data loss.
* **L3** : Edited merged daily products retaining all valid and good quality 
  measurements from all altimeters over one day (one daily file), with 
  simplified content (only a few key parameters). This is close to what is 
  delivered in NRT by CMEMS project.
* **L4** : Gridded products averaging valid and good measurements from all 
  available altimeters over a fixed resolution grid (1°x1°) on a monthly 
  basis. These products are meant for statistics, and visualisation through 
  the CCI toolbox.

These products are described in further details in the following sections, 
separated per type of instrument, altimetry or SAR.  

```{tableofcontents}
```
