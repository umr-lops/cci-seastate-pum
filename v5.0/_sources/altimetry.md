---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
mystnb:
  execution_timeout: 360
---

# Nadir-altimetry datasets

The main geophysical parameter provided by altimeters for sea state is the 
significant wave height (SWH). Three kinds of datasets are delivered, as 
summarized in table {numref}`altimeter_datasets`

```{table} Summary description CCI Sea State Altimeter datasets
:name: altimeter_datasets

| Product             | L2 Pre-Processed {numref}`l2p` | L3 Multi-Mission {numref}`l3` | L4 Multi-Mission {numref}`l4`                                             |
| ------------------- | ------------------------------ | ----------------------------- | ---------------------------- |
| Acronym             | L2P                                                                                                                                                                                                                                                                                        | L3                                                                                 | L4                                                                                 |
| Description         | SWH derived from Level 1 source data (wave forms) for a single mission but averaged at 1 Hz, typically in a satellite projection with geographic information. These expert data form the fundamental basis for higher-level products and require ancillary data and uncertainty estimates. Wind derived from altimeter radar backscatter are also provided.| SWH measurements from L2P of different missions stitched together into daily files | SWH measurements combined from a multiple missions into a monthly space-time grid. | 
| Grid specification  | Along-track observations                                                                                                                                                                                                                                                                   | Along-track observations                                                           | Global regular lat-lon grid, 1°x1°                                                 |                                                                                                                                            
| Temporal resolution | 1 second averages along full or half orbits                                                                                                                                                                                                                                                | 1 second averages into daily files                                                 | Monthly                                                                            |                                                                                                                                    
| Spatial resolution  | 6-7 km |  6-7 km | 1°x1° |
| Coverage            | Mission specific                                                                                                                                                                                                                                                                           | Native to data stream                                                              | Global                                                                             | 
```

## L2P

L2P Along-track files from altimetry missions provide altimeter cross-calibrated SWH and wind
measurements separated per satellite and pass (half-orbit), sometimes full orbit or 
sections of orbit (depending on the source provider for the altimeter data), 
including all measurements with flags, corrections and extra parameters
from other sources. These are expert products with rich content and no data loss.

```{figure} images/cci_l2p_altimeter.png
---
height: 250px
name: cci_l2p_altimeter
---
Example of a L2P coverage
```

The SWH and wind measurements are provided in two separate NetCDF file collections, 
as the cross-calibrated wind is computed for a limited set of missions only and not 
always over the whole mission lifetime because of inconsistent input backscatter 
measurements.

{numref}`l2p_altimeters` below summarizes the included missions for 
each type of L2P.

```{table} Summary description of the altimeters included in each L2P collection
:name: l2p_altimeters

| L2P collection | Missions |
| -------------- | -------- |
| SWH | ERS-1, ERS-2, ENVISAT, GFO, TOPEX-Poseidon (both altimeters), Jason-1, Jason-2, Jason-3, SARAL, Cryosat-2, Sentinel-3 A, Sentinel-3 B, Sentinel-6 A, SWOT/Nadir, CFOSAT |
| Wind | TOPEX-Poseidon (TOPEX only), Jason-1, Jason-2, Jason-3, Sentinel-3 A, Sentinel-3 B, Sentinel-6 A |
```



## L3 

Edited merged daily dataset retaining all valid and good quality SWH 
measurements from all L2P altimeters over one day (one daily file), with 
simplified content (only a few key parameters). This is close to what is 
delivered in NRT or Multi-Year time series by CMEMS project[^footnote1].

```{figure} images/cci_l3_altimeter.png
---
height: 250px
name: cci_l3_altimeter
---
Example of a multi-mission L3 coverage 
```

## L4

Gridded products averaging valid and good SWH measurements from all available
altimeters over a fixed resolution grid (1°x1°) on a monthly basis. This 
dataset is meant for statistics, and visualisation through the CCI toolbox.

```{figure} images/cci_l4_altimeter.png
---
height: 250px
name: cci_l4_altimeter
---
Example of a multi-mission L4 coverage 
```

[^footnote1]: https://doi.org/10.48670/moi-00176

