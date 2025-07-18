# ESA Sea State CCI datasets in a nutshell

## Where do I get the data?

Simple registration at https://forms.ifremer.fr/lops-siam/access-to-esa-cci-sea-state-data/
gives immediate access to ftp account details to access a **preliminary 
version** {{cci_version}} dataset referred to as "*interim*", located in 
`data/v4interim` folder. This release is called *“interim”* because a few months
of L2P data are still being completed or corrected for some of the missions, 
and L3/L4 datasets will be released a bit later. 

```{important}
The official release of {{cci_version}} will be accessible from ESA CCI 
Sea State Project web page at: https://climate.esa.int/en/projects/sea-state/data/. 
```


## What are the data products?

For **synthetic aperture radar (SAR)**, only L2P products are provided, 
consisting in sea state parameters observations over the radar swath separated 
into satellite directories, including all measurements with flags, corrections
and ancillary parameters from other sources.

For **altimetry**, three products are delivered:
- L2P : Along-track products separated into satellite directories, including 
  all measurements with flags, corrections and ancillary parameters from 
  other sources. These are expert products with rich content and no data 
  loss. Provided in separate directories for altimeter and SAR-derived products. 
- L3 : Edited merged daily products retaining only quality-checked 
  measurements from all altimeters over one day (one daily file), with simplified content (only a few key parameters). This type of content is close to what is delivered in NRT by the CMEMS project.
- L4 : Monthly gridded products averaging quality_checked measurements from 
  all available altimeters over a fixed resolution grid (1°x1°). These products are meant for statistics and visualization through the CCI toolbox.

```{note}
L2P and L3 products are most useful for the validation and or assimilation into 
numerical models: along-track data cannot resolve the fast variability of the 
sea state and therefore miss many wave events. The L2P and L3 data can also 
be used for long-term statistics, but the L4 product may be more suitable 
for these applications. 
```

## What is the difference between altimeter and off-nadir SAR data?

Altimeters measure the significant wave height (Hs) all along the satellite 
track, covering both oceans, inland seas and lakes. The altimeter 
measurement corresponds to a very narrow footprint (2-10 km, depending on 
the wave height and satellite altitude). In the Sea State CCI project we 
provide a homogeneous historical processing that uses Low Resolution Mode 
(LRM) in the case of satellites that also operate in Delay-Doppler model 
(often abusively called “SAR mode”) .

Synthetic Aperture Radars (SARs) map the Earth surface at high resolution 
(pixels are typically less than 20 m. As a result SAR images resolve some of 
the wavelengths of the sea state and the unresolved waves also contribute to 
the properties of the image. Combining resolved and unresolved information 
allows estimates of the following sea state parameters: $Hs$, mean periods 
$T_{m0,-1}$ (often called the energy period), first moment $T_{m0,1}$, second 
moment mean period $T_{m0,2}$ (close to the zero crossing period), dominant 
swell wave height $SW1$, secondary swell wave height $SW2$, and windsea 
wave height $SWW$ and windsea mean period $TMW$. These 8 parameters are 
provided for all SAR data. In addition, swell partition parameters (height,
period, direction) are provided as an experimental product for Sentinel 1 Wave 
Mode. 


## Coverage, resolution and the acquisition modes of SAR instruments

Most SAR instruments toggle between different acquisition modes designed to 
optimize coverage and resolution, and each mode has a different sampling 
pattern. For example, Sentinel 1 A/B/C, use these 3 modes: 

- Wave mode (WV): each image is 20 km by 20 km, with one image every 100 km 
  on the side of the satellite track. This mode is generally used in the 
  middle of oceans.
- Interferometric Wide Swath Mode (IW): these are 250 km wide, and wave 
  parameters are provided on a 5 km resolution grid. This mode is generally 
  used within 300 km from land, and for coastal seas (North Sea, 
  Mediterranean Sea...)
- Extra Wide swath (EW): these are 400 km wide and typically used in high 
  latitudes where sea ice occurs. 


```{figure} /images/dlr_sar_fig1.png
---
name: intro_fig1
---
An example of S1-A and S1-B coverage with WV mode (red), IW mode (green) and 
EW mode (grey). The SM mode data (blue) is not used in Seastate CCI. 
```


## What is the data format?
All Seastate CCI use NetCDF4 Classic format. When appropriate, the data is 
on a grid: regular grid in latitude and longitude for the L4 data, and grid 
corresponding to the SAR image geometry for the SAR-derived L2P data for IW 
and EW modes.

## What is the time span?
The altimeter dataset covers the period August 1991 (starting with ERS-1) to 
December 2023. 

```{figure} /images/altimeter_timeline.png
---
name: intro_fig2
---
Overview of the time span of the different past, existing and future 
altimeters. The CCI Sea State datasets {{cci_version}} include among these 
missions: ERS-1, ERS-2, Topex-Poseidon , Jason-1, ENVISAT, Jason-2, CryoSat-2, 
SARAL, Jason-3, Sentinel-3 A, Sentinel-3 B and Sentinel-6.  
```


SAR-derived data starts in October 2014 for SAR IW mode with Sentinel 1A. 




Bjorn Tings, Andrey Pleskachevsky  DLR 2025

## What is the spatial resolution?
The L2P and L3 altimeter products are along-track with narrow swath and resolution of 7km.
The L4 gridded products have a spatial resolution of 1°.
For SAR data (https://sentiwiki.copernicus.eu/web/s1-products): 
Wave mode imagettes are approximately 20km/20km stamps every 100km. 
Wide swath mode is  5km grids approximately 

## I am interested in obtaining all the best quality wave height measurements for a particular region and am not concerned about quality control or which satellite the measurements are from.
The L3 product contains all the significant wave height measurements that are considered good quality, merged from all available instruments, and splitted per day.

## I am interested in obtaining quick statistics for large areas.
The L4 product contains a statistical summary of the wave height 
measurements at a spatial resolution of 1° and temporal resolution of 1 month. 

## Who do I contact for further information?
For general data enquiries contact the Ifremer data team: cersat@ifremer.fr

For application and science issues contact Guillaume Dodet: guillaume.dodet@ifremer.fr 