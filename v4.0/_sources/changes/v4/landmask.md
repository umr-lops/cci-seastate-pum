# Landmask based on distance to coast

In the **ceraux** library, a distance to the coast model is already in place : the **GSFCDist2Coast** class, which allows to obtain the distance to the coast using a NASA/GSFC distance grid of 1km .
This requires a NetCDF or OpenDAP version of the distance file : http://oos.soest.hawaii.edu/thredds/dodsC/dist2coast_1deg_ocean.html

A new 300m grid (Globolakes) was found, offering better resolution.
This requires a NetCDF version of the distance file : https://data.ceda.ac.uk/neodc/globolakes/data/v1/limnology

A new class was therefore created: **GlobolakesDist2Coast**, in order to evaluate the differences with the 1km grid, and to determine the most appropriate solution.

It has been observed (as can be seen below) that the 300m grid does not detect all land fragments, particularly small atolls, which can be problematic and distort the data.

```{image} ./images/dist2coast_ex1.png
:name: dist2coast_ex1
```

```{image} ./images/dist2coast_ex2.png
:name: dist2coast_ex2
```

Therefore, the **GSFCDist2Coast** class is used at the moment.

Until now, the **average_to_1hz** method worked with creating a landmask using OpenStreetMap files. Now, this method can also work with the distance to the coast, thanks to the **land.py** file which allows to choose one or the other depending on what you tell it.
To do this, you must indicate in the configuration file the method used. For example :
- **GSFC** for the distance to the coast (1km grid)
- **OSM** for the landmask
- **GLOBOLAKES** for the distance to the coast (300m grid)

A distance threshold from the coast has also been added by default : only datas greater than 500m are kept.

The configuration allows you to choose between different calculation methods, depending on the specific needs of the project. It is crucial to properly configure the configuration file to ensure optimal use of the data and available resources.

In summary, the 1km grid (class **GSFCDist2Coast**) remains favored for the moment in the average_to_1hz method, due to its more complete detection capabilities.