# SAR datasets

The main geophysical parameter provided by SAR for sea state is the significant wave height (SWH).
Also, first and second moment wave periods, mean wave period, dominant and secondary swell wave heights and
windsea wave height and periods (total eight parameters). 

Three kinds of datasets are delivered, processed from Sentienl-1 three modes: 
•	Wave mode (WV) acquired over open oceans, along-orbit imagettes ca. 20×20 km every 100 km.
•	Interferometric Wide Swath mode (IW) in shelf regions and seas, strips up to 2000 km with swath width of ca. 250 km.
•	Extra Wide Swath mode (EW) in polar regions with swath width of ca. 450 km.

During processing at Ground Stations (GS) the IW and EW raw data are divided into images with individual product
IDs with along-flight length of ca. 200 km for IW and ca. 400 km for EW and converted into Level-1 (L1) products 
for easier distribution. This dividing can differ by processing of the raw SAR data at different GTs.

In total, pear a day ca.
 500 IW products 
 200 EW products 
  60 WV tracks (means ca. 600 imagettes)
were acquired (S1A and S1B).


```{table} DLR processing ocean products  
:name: dlr_products

| 1 ID product                    |  Processing        | 
|---------------------------------|---------------------------|
|S1 IW  coverage ca. 250x200 km | 5 km raster - ca. 1500 values/image |
|S1 EW coverage ca. 450x400 km  | 1.5 km raster - ca. 450 values/image  | 
|S1 WV coverage ca. 20x20 km, each 100 km along.track  | averaged values per imagette |  

```


```{figure} /images/dlr_sar_fig01.png
---
name: dlr_sar_fig01
---
An example of one-day sea state estimated from Sentinel-1 satellites S1-A and S1-B on 2021-01-01.
The three main acquisition modes WV along-flight imagettes, IW, EW scenes are acquired in different areas with switched acquisition modes. 
The acquired IW and EW raw data are divided into individual products with along-flight length of ca. 200 km for IW and ca. 400 km for EW.
In total, 748 products with ca. 570,000 processed water points are shown: 59 WV tracks (averaged values for an imagette 20×20 km),
513 S1 IW products (5 km processing raster), 212 EW products (ca. 17.5 km processing raster).

```