# Datasets

Three kinds of datasets are delivered, as summarized in table 
{numref}`altimeter_datasets`

```{table} Summary description CCI Sea State Altimeter datasets
:name: altimeter_datasets

| Product             | L2 Pre-Processed {numref}`l2p`                                                                                                                                                                                                                                                             | Multi-Mission Along-Track {numref}`l3`                                             | Multi-Mission Statistics  {numref}`l4`                                             |
|:--------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------|
| Acronym             | L2P                                                                                                                                                                                                                                                                                        | L3                                                                                 | L4                                                                                 |
| Description         | SWH derived from Level 1 source data (wave forms) for a single mission but averaged at 1 Hz, typically in a satellite projection with geographic information. These expert data form the fundamental basis for higher-level products and require ancillary data and uncertainty estimates. | SWH measurements from L2P of different missions stitched together into daily files | SWH measurements combined from a multiple missions into a monthly space-time grid. | 
| Grid specification  | Along-track observations                                                                                                                                                                                                                                                                   | Along-track observations                                                           | Global regular lat-lon grid, 1°x1°                                                 |                                                                                                                                            
| Temporal resolution | 1 second averages along full or half orbits                                                                                                                                                                                                                                                | 1 second averages into daily files                                                 | Monthly                                                                            |                                                                                                                                    
| Coverage            | Mission specific                                                                                                                                                                                                                                                                           | Native to data stream                                                              | Global                                                                             | 
```



## Retracking

### Retracking of conventional altimeters

In CCI SeaState version 1 Dataset, significant wave height was taken from
agencies GDR products. In version 2 and later of CCI Sea State Dataset, we
have performed a complete retracking of the included missions from the wave forms
available in input SGDR products (Level 1) provided by space agencies. The
retracking calculates 20 Hz SWH measurements (40 Hz for Saral) from the
waveforms. TUM’s retracker **WHALES** was used for all non-SAR altimetry
missions (Envisat, CryoSat-2, Saral, Jason-1, Jason-2, Jason-3).

The Low Resolution Mode (LRM) waveforms are characterised by a rising leading
edge that becomes less steep as the SWH increases, and a slowly decreasing
trailing edge. The standard retracking methods are still affected by a
suboptimal distribution of the residuals in the fitting process, which results
in high level of noise in the estimations. WHALES is designed as a unified way
to solve these problems and is based on two principles:

- The application of a weighted fitting solution, whose weights are adapted to
  the SWH in order to guarantee a more uniform distribution of the residuals
  during the iterative fitting. This guarantees significantly more precise
  estimations.
- A subwaveform strategy to focus the retracking on the portion of the signal of
  interest, avoiding heterogeneous backscattering in the trailing edge
  (partially inherited from the ALES retracker, Passaro et al., 2014). This
  guarantees efficiency in the coastal zone and a better representation of the
  oceanic scales of variability.

Moreover, a revisiting of the look-up tables used to correct for the Gaussian approximation of
the Point Target Response in the Brown model ensures that the accuracy in the estimation is
tailored to the new retracking solution.

References:
Passaro M., Cipollini P., Vignudelli S., Quartly G., Snaith H.: ALES: A multi-mission
subwaveform retracker for coastal and open ocean altimetry. Remote Sensing of
Environment 145, 173-189, 10.1016/j.rse.2014.02.008, 2014

