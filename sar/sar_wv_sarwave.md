# L2P "SARWAVE" SAR Wave Mode by Ifremer

This section covers the wave height estimation from Envisat and Sentinel-1
wave mode processor.

SARWAVE Level-2 products are obtained from SAR level-1 SLC data acquired in 
Wave Mode with C-band SAR on board Envisat and Sentinel-1 missions. A Level-2 
product is provided for each Level-1 product used as input of the SARWAVE 
processor.

The main processing steps are:
- the computation SAR image cross-spectra from these looks [Engen]
- the computation radar parameters from the cross-spectra 
  (Schulz-Stellenfleth et al., 2007, Li et al., 2019)
- the use of a transfer function to provide estimates of the significant wave 
  height, the wind sea significant wave height, averaged period (T0m1) and 
  Tm2 (Stopa and Mouche, 2017)


## Processing details

### Image cross-spectra
SAR image cross-spectra are computed from the level-1 SLC product and are key 
input in the algorithm for wave retrieval. 

The main steps to compute the image cross-spectra are following the approach 
described by Engen and Johnsen (1995). A cross-spectrum between two Looks $i$ 
and $i+n$ writes:

$$XS^{n\tau} (f_{rg},f_{az}) = FT^{2D}[Look^i] \bullet FT^{2D}[Look^{i+n}]^*$$

where $FT^{2D}$ is the 2D Fourier Transform and $f_{rg}$ and $f_{az}$ 
respectively defined as the range and azimuth spatial frequencies.

The state-of-the-art definition of look is defined as a scene of a given 
area whose resolution is achieved during a certain time duration defined as 
a portion of the total azimuthal Doppler bandwidth. The Looks can also be 
defined with an overlap in the time domain also defined as a portion of the 
total azimuthal Doppler bandwidth.

### CWAVE radar parameters
Among the key variables defined to build a robust transfer function between 
Level-1 SLC products derived parameters and Level-2 geophysical parameters, 
we make use of the particular moments of the cross-spectrum termed as the 
CWAVE parameters and introduced by Schulz-Stellenfleth et al., 2007. 

The spectral information of the normalized cross-spectrum is decomposed 
according to orthonormal functions $H_{ij}$ defined as tensor products of 
Gegenbauer polynomial $G_i(\alpha_k(k_x,k_y))$ and harmonic $F_j(\alpha_\phi
(k_x,k_y))$ functions defined in the azimuth k_y and range k_x wave-number 
space. This yields to the general formulation of CWAVE parameters $C_{ij}$:

$$C_{ij} = \sum_{k_x,k_y} \bar P (k_x,k_y)H_{ij}(k_x,k_y)dk_xdk_y$$

with $i$ in $[1,n_k]$ and $j$ in $[1,n_{\phi}]$. In this study $n_k=4$ and 
$n_{\phi}=5$. The orthonormal functions are defined in Stopa and Mouche (2017).  

### IMACS radar parameters
Among the key variables defined to build a robust transfer function between 
Level-1 SLC products derived parameters and Level-2 geophysical parameters, 
we make use of the particular moment of the cross-spectrum computed on the 
high frequency part of domain close the range axis, termed as the MACS 
parameter and introduced by Li et al., 2019.

$$MACS(\bar k,\Delta t) = \frac{1}{N} \sum_{n=1}^{N}P_s^{(m,n)}(k_{az},k_{ra}
,\Delta T),    (k_{az},k_{ra})\in \mathbf A$$

For Sentinel-1 Wave Mode data, Li et al. (2019) defined the wavenumber 
domain area A to wavenumbers lower than 2π/600 rad·m−1  in the azimuth 
direction and over intermediate range-detected waves, corresponding to 
filtering the cross-spectra around range (across-track) wavelength between 
15 m and 20 m. The lower limit of range wavelength is set to be 
approximately three times the nominal S-1 line-of-sight ground resolution. 
The domain A shall be different for Envisat/ASAR data. This still needs to 
be defined.

### Transfer function
A transfer function (Trans. Func. or GMF for geophysical Model Function) 
relates $M$ radar-derived parameters $X$ to $N$ geophysical quantities $Y$:

$Y_{i\in [1,N]} = GMF(X_{j\in [1,M]})$

For the proposed transfer function there are 24 input parameters $X$ listed 
below:
- denoised normalized radar cross-section,
- normalized variance,
- incidence angle,
- azimuth cutoff,
- 20 CWAVE parameters,

The target values $Y$, extracted from a wave model, are:
- the significant wave height,
- the fraction of wave height due to wind sea, 
- the mean wave period T0m1. 
- Tm2

We propose a deep learning model to derive the transfer function. The model 
is composed of 10 dense layers of 1024 neurons with rectified linear unit 
(ReLU) activation function. The output layer is composed of 3 parallel dense 
layers of size 200 that receive a softmax activation function to ensure the 
values in each class sum up to 1. Thus, the model outputs a probability 
distribution for each sea state parameter. The model is trained using 
the cross-entropy loss.

```{admonition} References
:class: note

Engen, G. and Johnsen, H. (1995). Sar-ocean wave inversion using image cross 
spectra. IEEE Transactions on Geoscience and Remote Sensing, 33(4):1047–1056.

Li H., Chapron B., Mouche A., Stopa J. (2019). A new ocean SAR 
cross-spectral parameter: definition and directional property using the 
global Sentinel-1 measurements . Journal Of Geophysical Research-oceans , 124
(3), 1566-1577

Stopa, J. E. and Mouche, A. (2017). Significant wave heights from sentinel-1 
sar: Validation and applications. Journal of Geophysical Research: Oceans, 
122(3):1827–1848.

Schulz-Stellenfleth, J., König, T., and Lehner, S. (2007). An empirical 
approach for the retrieval of integral ocean wave parameters from synthetic 
aperture radar data. Journal of Geophysical Research: Oceans, 112(C3).
```
