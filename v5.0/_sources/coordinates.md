---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
---

(coordinate_variables_all)=
# Coordinate variables
NetCDF coordinate variables provide scales for the space and time axes for the
multidimensional data arrays, and are included for all dimensions that can 
be identified as spatio-temporal axes. 

## Temporal coordinates

`time` is the time coordinate of the CCI Sea State datasets. It is computed to 
the nearest second at least and then coded as continuous UTC time coordinates in 
**seconds from 00:00:00 UTC January 1, 1980** (which is the definition of the
CCI Sea State origin time, chosen to precede the start of useful altimeter or 
SAR data record).

For L2P and L3 datasets, `time` is defined for each measurement within the 
data files. For L4 datasets, a single time value is provided, corresponding to 
the centre time of the averaging or collation window. In both case, the time 
values are defined along the `time` axis dimension.

```
double time(time) ;
    	time:_FillValue = 1.e+20 ;
    	string time:long_name = "reference time of sst file" ;
    	string time:standard_name = "time" ;
    	string time:authority = "CF-1.12, ACDD-1.3" ;
    	string time:axis = "T" ;
    	string time:coverage_content_type = "coordinate" ;
    	string time:units = "seconds since 1981-01-01" ;
    	string time:calendar = "proleptic_gregorian" ;
```


## Spatial coordinates

`lat` and `lon` variables provide respectively the latitude and longitude of 
each measurement within CCI Sea State dataset files. Latitudes are expressed 
between -90° and +90° and longitudes between -180° and 180°.


A `coordinates` variable attribute (`coordinates = "lon lat" ;`) is 
provided if the data are on a non-regular lat/lon grid (SAR image, swath or 
along-track data).

A `grid_mapping` variable attribute (`grid_mapping = "<projection name>" ;`) 
is provided if the data are mapped following a projection. Refer to the 
CF convention[^footnote2] for standard projection names.

### Along-track latitude/longitude (nadir altimeter)

For altimeter along-track data (L2P, L3), the latitude and longitude values are 
defined for each measurement along the `time` axis dimension.

```
double lon(time) ;
    	string lon:long_name = "longitude: 1 Hz" ;
    	string lon:units = "degrees_east" ;
    	string lon:standard_name = "longitude" ;
    	string lon:comment = "geographical coordinates, WGS84 projection" ;
    	string lon:authority = "CF-1.11, ACDD-1.3" ;
    	string lon:axis = "X" ;
    	lon:valid_range = -180., 180. ;
    	string lon:coverage_content_type = "coordinate" ;
double lat(time) ;
    	string lat:long_name = "latitude: 1 Hz" ;
    	string lat:units = "degrees_north" ;
    	string lat:standard_name = "latitude" ;
    	string lat:comment = "geographical coordinates, WGS84 projection" ;
    	string lat:authority = "CF-1.11, ACDD-1.3" ;
    	string lat:axis = "Y" ;
    	lat:valid_range = -90., 90. ;
    	string lat:coverage_content_type = "coordinate" ;
```

### Non-regular latitude/longitude grids (swath, image)

For non-regular gridded data following the sensor scanning pattern (such as SAR 
images), **x** (columns) and **y** (lines) grid dimensions are referred 
either as `col` (for the across-track dimension) and`row` (for the 
along-track dimension).

In this case where data are gridded following the sensor pattern, no projection 
can be associated and lat/lon data have to be stored in 2-D arrays, as it is 
the case for along-swath data for low earth orbiting sensors. Therefore it only 
applies to **L2P** processing level products.

```
double lat(row, col) ;
    	lat:_FillValue = 1.e+20 ;
    	lat:standard_name = "latitude" ;
    	lat:long_name = "latitude" ;
    	lat:units = "degrees_north" ;
double lon(row, col) ;
    	lon:_FillValue = 1.e+20 ;
    	lon:standard_name = "longitude" ;
    	lon:long_name = "longitude" ;
    	lon:units = "degrees_east" ;
```


### regular latitude/longitude grids

For orthogonal regular geographic (lat-lon) gridded data (L4), **x** 
(columns) and **y** (lines) grid dimensions are referred either as `lat` and 
`lon`. Coordinate vectors are used. The elements of a coordinate vector array 
are in monotonically increasing or decreasing order.

```
double lon(lon) ;
    	string lon:long_name = "longitude" ;
    	string lon:units = "degrees_east" ;
    	string lon:standard_name = "longitude" ;
    	string lon:comment = "geographical coordinates, WGS84 projection" ;
    	string lon:authority = "CF-1.12, ACDD-1.3" ;
    	string lon:axis = "X" ;
    	lon:valid_range = -180., 180. ;
    	string lon:coverage_content_type = "coordinate" ;
double lat(lat) ;
    	string lat:long_name = "latitude" ;
    	string lat:units = "degrees_north" ;
    	string lat:standard_name = "latitude" ;
    	string lat:comment = "geographical coordinates, WGS84 projection" ;
    	string lat:authority = "CF-1.12, ACDD-1.3" ;
    	string lat:axis = "Y" ;
    	lat:valid_range = -90., 90. ;
    	string lat:coverage_content_type = "coordinate" ;
```


[^footnote2]: http://cfconventions.org/cf-conventions/cf-conventions.html#appendix-grid-mappings