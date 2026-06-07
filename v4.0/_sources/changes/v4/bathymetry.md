# Fixing and updating bathymetry

The handling of bathymetry information is handled in the class 
``GEBCO14Bathymetry`` from [ceraux](https://gitlab.ifremer.fr/cerbere/ceraux/) 
package.

We notice that in **version 3** of CCI Sea State dataset there is a problem 
with negative longitudes:

```{image} ./images/bathymetry_problem.png
:name: bathymetry_problem
```

It was found out that there was an error in the bathymetry grid vector decoding
(read in -180°/180° convention instead of 0°/360° convention). Correcting for 
this shift in ``GEBCO14Bathymetry`` class, the problem is now resolved :

```{image} ./images/bathymetry_resolution.png
:name: bathymetry_resolution
```

In the mean time, a new version **GEBCO 2024** was made available (this time 
-180°/180° convention!) with an improved 15'' resolution (instead of 30'') 
**which is now used instead**, through a new ``GEBCO24Bathymetry`` class in 
[ceraux](https://gitlab.ifremer.fr/cerbere/ceraux/) 

