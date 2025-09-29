# L2P  SAR 

The SAR L2P products are:
- along-track files for S1 wave mode WV (averaged values for each 20x20 km imagette acquired each 100 km) 
- sea state rastered fields with mesh of 5 km for S1 IW and 17.5 km for S1 EW.


## L2P  SAR L2P geophysical data record format specification

The Table provides an overview of the CCI Sea State L2P environment (geophysical) data record within a L2P file (DLR_OCN).

```{table} Summary description of CCI Sea State L2P geophysical data records SAR (DLR_OCN)  
:name: dlr_products
|                     |       Abb. in netCDF product    | Description in netCDF product                               |       Unit/Meaning                       |
|---------------------|---------------------------------|-------------------------------------------------------------|------------------------------------------|
| total significant wave height    | swh                | total C band significant wave height                        | m                                        | 
| mean wave period   Tm0-1         | Tm0                | C band Mean wave period                                     | s                                        | 
| first moment wave period         | Tm1                | C band First moment period                                  | s   
| second moment wave period        | Tm2                | C band Second moment period                                 | s   
| wave height swell dominant system | swell_swh_primary | C band Dominant swell wave height                           | m   
| wave height swell secondary system |	well_swh_secondary  | C band Secondary swell wave height                      | m   
| significant wave height windsea  |	windwave_swh    |C band Windsea wave height                                   | m   
| mean period windsea |	windwave_period                 | C band Windsea wave period                                  | s   
| quality flag        | swh_quality                     | quality of C band SAR significant wave height measurement   | 0=undefined (e.g. land),    1=bad,   2=acceptable (not used),    3 – good |
| rejection flag      | swh_rejection_flags             | consolidated Significant Wave height quality flags          | 1=Nv (variance) > max (not more used), 2=swh outlier, 4=invalid value, 8=wind below 2 m/s (not more used),  16=not_water (land) |
| uncertainty SWH     | swh_uncertanty                  | best estimate of significant wave height standard error     | to MFWAM (CMEMS) estimated for each swh domains 0-1.5 m, 1.5-3m, 3-6m, >6m and interpolated/extrapolated |
| uncertainty SW1     | swell_swh_primary_uncertanty    | Best estimate of dominant swell wave height standard error  | to MFWAM (CMEMS) estimated for each swh domains 0-1.5 m, 1.5-3m, 3-6m, >6m and interpolated/extrapolated |
| uncertainty SW2     | swell_swh_secondary_uncertanty  | Best estimate of secondary swell wave height standard error | to MFWAM (CMEMS) estimated for each swh domains 0-1.5 m, 1.5-3m, 3-6m, >6m and interpolated/extrapolated |
| uncertainty SWW     | windwave_swh_uncertanty         | Best estimate of windsea wave height standard error         | to MFWAM (CMEMS) estimated for each swh domains 0-1.5 m, 1.5-3m, 3-6m, >6m and interpolated/extrapolated |
| uncertainty Tm0     | Tm0_uncertanty                  | Best estimate of mean wave period standard error            | to MFWAM (CMEMS) estimated for each swh domains 0-4 s, 4-7 s, 7-10s, >10 s and interpolated/extrapolated |
| uncertainty Tm1     | Tm1_uncertanty                  | Best estimate of first moment wave period standard error    | to WW3 estimated for each swh domains 0-4 s, 4-7 s, 7-10s, >10 s and interpolated/extrapolated |
| uncertainty Tm2     | Tm2_uncertanty                  | Best estimate of second moment wave period standard error   | to MFWAM (CMEMS) estimated for each swh domains 0-4 s, 4-7 s, 7-10s, >10 s and interpolated/extrapolated |
| uncertainty Tmw     | windwave_period_uncertanty      | Best estimate of mean period windsea standard error         | to MFWAM (CMEMS) estimated for each swh domains 0-4 s, 4-7 s, 7-10s, >10 s and interpolated/extrapolated |
```
## L2P  SAR L2P quality_level

The DLR algorithm SAR-SeaStaR is optimised for minimizing the percenatge of the non-valid data. For example, the number of non-valid ocean imagettes for S1 WV is ca. 1.5% with an accuracy of ca. 0.26 m in terms of SWH. 
This optimization allows processing data for ambiguous enviroment conditions dominate in coastal ares (NRCS artifacts by ships, buoys, watermarks, windparks, etc.) and by low winds under 2 m/s.

Due to series of control procedures, rejection has been inicialized:
 - =0  -  undefined (e.g. land)
 - =1  -  bad
 - =2  -  acceptable (not used),
 - =3  -  good 

## L2P  SAR L2P rejection_flags

 - =1  -  variance > max (not more used)
 - =2  -  swh outlier, non-valid 
 - =4  -  invalid value
 - =8  -  valid value, wind below 2 m/s (not more used)
- =16  -  not_water (land), correspondes to quality_level=0 (land) 







 - invalid value (swh_quality=4)
 - valid value can have lower accuracy due to geophysical ambiguous enviroment (rejection_flag=8 for wind below 2 m/s). 



