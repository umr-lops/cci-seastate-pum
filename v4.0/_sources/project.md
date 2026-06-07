# The CCI Sea State project

## Sea state for climate

The statistical properties of ocean waves (average heights, periods, 
directions...) are known as sea state and their measurements are used in 
coastal and ocean engineering (e.g. for defining heights and periods with a 
given return period). 

```{image} /images/overview1.png
:width: 150px
:align: right
```

SeaState is an Essential Climate Variable that impacts:  
- navigation safety 
- any operations at sea (energy production) or on the coast
- Long term and consistent time series are critical for managing ocean and 
  coastal infrastructure, and planning for the future (adaptation…)

The key questions it raises are : 
- what is the wave climate today? (especially for extremes)
- what interannual variability and trends can we expect?
- How do we explain these variations? (winds, currents, sea ice … ) 

And the Central and specific work consists of: 
- estimating measurement uncertainties, 
- inter-calibration methods and uncertainties
- sampling issues: complementarity of models & satellite data


## Project overview 

The main objective of this Research & Development project is to provide a 
consistent and long-term time series of sea-state parameters. In practice 
the ongoing work is the second phase of the Sea state CCI project, and 
builds upon methods and dataset already produced as part of the first phase. 
Our work is thus an upgrade of the V3 version (last delivery of phase 1 
which covered 2000-2022), and merger with V1 (which covered 1991 to 2008).  
This project will deliver 2 versions: V4 in 2025 and V5 in 2026, including 
different time spans, instruments, missions, variables... 

We start from observation requirements laid out by 
[GCOS](https://gcos.wmo.int/en/home), and the needs of both the climate 
research community, which are particularly laid out in the 
[COWCLIP](https://cowclip.org/) publications, and key applications 
communities that include coastal management and shipping. These requirements 
will be refined and evolved as part of the project.

Of primary concern is the accuracy and stability of the data set that 
produce. These include:
- Significant wave height from satellite altimeters that include all the ESA 
  missions, and Topex/Jason series.
- Wave spectra from radar imagery: all wave mode from ERS to Sentinel 1 and a 
  sample of larger image mode data. Including Partition Test data set from 
  Sentinel-1 (ODL)
- Wide swath SAR Imager data : 
  - Interferometric (IW) + ExtraWide Swath (EW) (DLR)
  - SAR Envisat ASAR (DLR)

Sea state has never been the main focus of satellite altimetry. Except for 
the results of the Globwave project and new 
[Copernicus Marine Service activities](https://marine.copernicus.eu/), wind 
and wave parameters from altimeters were 
a by-product of sea level estimates, leading to inconsistencies between 
sensors and satellite missions. Our project will thus carry out algorithm 
development to ensure the best possible quality of wave and wind products 
with a particular focus on consistency.

A particular challenge is posed by the strong evolution of measurement 
techniques, in particular going from Range-only to Delay-Doppler processing, 
and we will work towards harmonizing the processing to arrive at seamless 
products than span the years 2003 to 2024 as part of this project, and 
making possible longer time series. The data production system will be fully 
documented, making future transfer possible, notably to the C3S service for 
the most mature time series.

The project will provide time series of wave parameters (by mission - level 
2, and consolidated - level 3) and gridded analyses of wave climate 
parameters (level 4). Based on these products we will work with the Earth 
observation and climate science community to identify and better analyze key 
processes that contribute to uncertainties in other ECVs (primarily sea ice 
and sea level). We also worked in phase 1 with the seismology community to 
make better use of microseism data for wave event detection and sea state 
trends analyses. But for the phase2, this is not the main focus.

The Project results, from the new algorithms, to the data set and their 
application in key areas will published in world class peer-reviewed 
scientific journals, in particular in time to contribute to IPCC Assessment 
Reports.

### User requirements gathering 

Answering end users' needs is one of our main concerns. Regularly, User 
Requirement Documents compile the synthesis of who are the users, what they 
require and how they use the datasets. User Consultations Meeting was also 
organised in phase 1 and will be renewed for phase 2 in order to link the 
upstream developments and the downstream uses.

{numref}`survey` sums up the results of a survey sent to a large community of 
Wave interested people around the world (100 answers around 18 countries).

```{figure} /images/overview3.png
---
name: survey
---
Distribution among the field of applications
```

### The future 
Our main concern for the moment is to identify relevant metrics related to 
climatic evolution of the wave characteristics (monthly average evolution, 
number of extreme events…).

Starting with their energy (Significant Wave Height) we plan to expand the 
analysis to other parameters (spectral width, wavelength, direction…) 
derived from different types of sensors...

The idea is then (in Version V5) to explore other complementary types of 
observations (diffusiometer, XBand SAR…) and variables (Wind…).

A strong effort is also made to define relevant uncertainties associated 
with each datasets.

The most mature algorithms, data series and missions are then expected to be 
pushed towards (Copernicus Climate Change Service) that will prolongate
operationally  the time series on the flow.   
 
```{image} /images/overview3.png
```
