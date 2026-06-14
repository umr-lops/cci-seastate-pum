(naming)=
# Naming conventions

All products are in NetCDF4 Classic format. They comply to the [CCI Data 
Standard 2.2](http://cci.esa.int/sites/default/files/CCIDataStandards_v2-2_CCI-PRGM-EOPS-TN-13-0009.pdf) 
and follow the [CF 1.12](https://cfconventions.org/) and [ACDD 1.4](https://wiki.esipfed.org/Attribute_Convention_for_Data_Discovery_1-3) 
conventions.

The file nomenclature is based on **form 2** of CCI recommendations :

`ESACCI-<CCI Project>-<Processing Level>-<Data Type>-<Product String>[-<Additional Segregator>]-<IndicativeDate>[<Indicative Time>]-fv<File version>.nc`

Where:
* `<CCI Project>` : *SEASTATE*
* `<Processing Level>` : here *L2P* for along-track data, *L3* for edited 
  merged product and *L4* for monthly averages
* `<Data Type>` : *SWH* for “Significant Wave Height”, *ISSP* for “Integrated 
  Sea State Parameters”, *WND* for "Wind speed"
* `<Product String>` : contains the name of the mission for L2P
* `<Additional Segregator>` : optionally the name of algorithm used
* `<Indicative Date>[<Indicative Time>]` : The identifying date for this 
  data set. Format is *YYYYMMDDTHHMMSS*, where *YYYY* is the four digit year, 
  *MM* is the two digit month from 01 to 12 and *DD* is the two digit day of 
  the month from 01 to 31. The date used should best represent the 
  observation date for the data set. For along-track data it will be the 
  time of the first measurement in the file.
* `<File version>` : File version number in the form *n{1,}[.n{1,}]* (That is 
  1 or more digits followed by optional . and another 1 or more digits.)


Examples:

* L2P altimeter product: `ESACCI-SEASTATE-L2P-SWH-Jason-2-20170130T145103-fv01.nc`
* L3 product: `ESACCI-SEASTATE-L3-SWH-MULTI_1D-20170130-fv01.nc`
* L4 product: `ESACCI-SEASTATE-L4-SWH-MULTI_1M-201701-fv01.nc`
