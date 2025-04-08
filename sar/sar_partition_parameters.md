# Sentinel-1 SAR Partition parameters datset

This page provides general information regarding the SAR partition parameter dataset produced by ODL.

## General description of dataset

The datset contains partition parameters obtained from Sentinel-1 derived SAR cross-spectra. 
The developed algorithm exploits artificial neural networks to reconstruct directional wave spectra from SAR-derived
cross-spectra. The reconstructed directional wave spectra are subsequently partitioned using a Watershed algorithm,
and partition parameters are computed for the detected partitions. The neural network was trained on 
Sentinel-1 cross-spectra and corresponding colocalised WaveWatchIII simulated directional wave spectra (the dataset used is the
Sentinel-1 WV L1C SARWAVE dataset produced by Ifremer).  

## Processed files

Currently, the dataset contains the results of processing the whole Sentinel-1 WV L1B SARWAVE dataset 
(https://cerweb.ifremer.fr/datarmor/tmp/proto_sarwave_L1B_doc/product_description.html), which
contains all Sentinel-1 observations that are colocalised with altimeter missions (which amounts to approximately 27% of the whole 
Sentinel-1 archive). The processed dataset comprises a total 3.120.750 NetCDF files, stored within 135.066 SAFE 
folders. The dataset spans the global ocean, from January 2015 to October 2023.

## Dataset file naming convention and organization

We adopt the SARWAVE file naming convention, which follows the netCDF file naming convention adapted from ESA Sentinel-1.
For the SARWAVE L1B dataset, each product is stored in a .SAFE directory, with the SAFE convention being inherited from the Sentinel-1 mission.
Each .SAFE directory contains 3 or 6 netCDF files, one per polarization (could be single polarization 
HH/VV or dual polarization HH/HV - VV/VH) plus one per sub-swath, following the official ESA SLC products storage 
convention. Importantly, the SLC (single look complex) acronym has been replaced by XSP (for cross-spectrum, which is the most important variable in the SARWAVE dataset)

Example of SARWAVE Level-1B measurement filename:

*l1b-s1a-iw1-vh-xsp-20240205t152312-20240205t152313-051412-063451-001-A02.nc*

where

- *l1b* stands for the processing level

- *s1a* gives the SAR unit name

- *iw1* gives the acquisition mode and the subswath number (up to 3 for IW and up to 5 for EW)

- *vh* is the polarisation of the acquisition: transmit in V and received in H in this example

- *xsp* is the 3-character defining the sub-product that can be found in the file

- *20240205t152312-20240205t152313* are respectively start and stop acquisition dates

- *051412* is the absolute orbit number

- *063451* is the mission datatake ID

- *001* is the image number (ie measurement) in the SAFE directory

- *A02* is a 3-character code that allow to track the processor used to generate the file, its version, and the processing options

To differentiate our files, we append the string '*_pparams_ODL_V1.0*' at the end of the file name.

## Variables in the Dataset

### Variables copied from Sentinel-1 L1B SARWAVE dataset

Each file in the Sentinel-1 L1B SARWAVE dataset contains a single real/imaginary cross-spectra pair, corresponding to a single Sentinel-1 imagette, 
as well as basic spatio-temporal localization and auxiliary variables (longitude, latitude, ground heading, incidence, tau, etc.).

To faciliate usage, the following variables were directly copied from the Sentinel-1 L1B SARWAVE dataset:

- *incidence*
- *ground_heading*
- *sensing_time*
- *tau*
- *azimuth_cutoff*
- *azimuth_cutoff_error*
- *longitude*
- *latitude*
- *land_flag* 

Information regarding these variables can be found on the Level-1B SAR Ifremer Product Description:
https://cerweb.ifremer.fr/datarmor/sarwave/documentation/processor/sar/xsarslc/html/product_description.html

### New variables

The following variables were produced by ODL by processing Sentinel-1 L1B SARWAVE cross-spectra
using the proposed methodology:

#### Dimensions and coordinates

```{table} Dimensions and coordinates
:name: dimensions_and_coordinates
| Parameter                    | Variable name | Unit   | Dimensions | Description                                                                         |
|------------------------------|---------------|--------|------------|-------------------------------------------------------------------------------------|
| Wavenumber                   | k             | rad/m  | (60,)      | Wavenumber vector for variables in polar coordinates                                |
| Direction                    | phi           | deg    | (72,)      | Direction vector for variables in polar coordinates (direction is CW from North)    |
| Partition number             | partition     | -      | (10,)      | Partiton number for partition label indexing (partitions sorted by peak wavenumber) |
```

#### Wave spectra reconstruction and partitioning

```{table} Wave spectra reconstruction and partitioning
:name: spectra_and_partitioning
| Parameter                  | Variable name    | Unit | Dimensions | Description                                                                                                         |
|----------------------------|------------------|------|------------|---------------------------------------------------------------------------------------------------------------------|
| Reconstructed wave spectra | wave_spectra     | m^4  | (phi,k)    | Wave spectra (in polar coordinates) reconstructed from SAR cross-spectra using the proposed neural network approach |
| Partition labels           | partition_labels | -    | (phi,k)    | Pixel-wise partition labels for reconstructed wave spectra (0 means no partition assigned)                          |
| Total number of partitions | n_partitions     | -    | Scalar     | Number of watershed detected partitions                                                                             |
```

#### Global parameters

```{table} Global parameters (for internal verification and validation)
:name: global_parameters
| Parameter                                | Variable name  | Dimensions | Unit  | Description                                                                 |
|------------------------------------------|----------------|------------|-------|-----------------------------------------------------------------------------|
| Golbal significant wave height           | global_swh     | Scalar     | m     | Significant wave height computed on the whole reconstructed spectra         |
| Global mean wave period (Tm0-1)          | global_Tm_0m1  | Scalar     | s     | Mean wave period (Tm0-1) computed on the whole reconstructed spectra        |
| Global second moment wave period (Tm2)   | global_Tm2     | Scalar     | s     | Second moment wave period (Tm2) computed on the whole reconstructed spectra |
```

#### Partition parameters

```{table} Partition parameters
:name: partition_parameters
| Parameter                                                                                     | Variable name                        | Unit  | Dimensions   | Description                                                                                                                           |
|-----------------------------------------------------------------------------------------------|--------------------------------------|-------|--------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Partition significant wave height                                                             | partition_swh                        | m     | (partition,) | Partition significant wave height                                                                                                     |
|-----------------------------------------------------------------------------------------------|--------------------------------------|-------|--------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Partition peak wavenumber                                                                     | partition_k_peak                     | rad/m | (partition,) | Partition peak wavenumber                                                                                                             |
| Partition peak direction                                                                      | partition_phi_peak                   | deg   | (partition,) | Partition peak direction (direction is CW from North)                                                                                 |
|-----------------------------------------------------------------------------------------------|--------------------------------------|-------|--------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Partition spectral spread (Gaussian fitting)                                                  | partition_k_spread                   | rad/m | (partition,) | Partition spectral spread computed by 1D Gaussian fitting on whole partition                                                          |
| Partition spectral spread at partition peak (Gaussian fitting)                                | partition_k_spread_peak              | rad/m | (partition,) | Partition spectral spread computed by 1D Gaussian fitting around partition peak                                                       |
| Partition spectral spread (spectral bandwith) using partition-restricted directional moments  | partition_k_spread_spectral          | rad/m | (partition,) | Partition spectral spread computed as spectral bandwith from Fourier directional moments on whole partition                           |
| Partition spectral spread (spectral bandwith)  using global directional moments               | partition_k_spread_spectral_global   | rad/m | (partition,) | Partition spectral spread computed as spectral bandwith from Fourier directional moments on whole spectra and restricted to partition |
|-----------------------------------------------------------------------------------------------|--------------------------------------|-------|--------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Partition directional spread (Gaussian fitting)                                               | partition_phi_spread                 | deg   | (partition,) | Partition directional spread computed by 1D Gaussian fitting on whole partition                                                       |
| Partition directional spread at partition peak (Gaussian fitting)                             | partition_phi_spread_peak            | deg   | (partition,) | Partition directional spread computed by 1D Gaussian fitting around partition peak                                                    |
| Partition directional spead using partition-restricted directional moments                    | partition_phi_spread_fourier         | deg   | (partition,) | Partition directional spread computed from Fourier directional moments on whole partition                                             |
| Partition directional spread using global directional moments                                 | partition_phi_spread_fourier_global  | deg   | (partition,) | Partition directional spread computed from Fourier directional moments on whole spectra and restricted to partition                   |

```






