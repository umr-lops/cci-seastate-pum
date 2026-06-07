# Data access

## Basic download
The data can be obtained from Ifremer through HTTP or FTP.

- for FTP, go to: ftp://ftp.ifremer.fr/ifremer/cersat/data/ocean-waves/cci-seastate/v4/
- for HTTP, go to: https://data-cersat.ifremer.fr/data/ocean-waves/cci-seastate/v4/

No login or password is required.

The data will be later pushed to the legacy ESA CCI archive at: https://climate.esa.int/en/projects/sea-state/data/ .

The common directory structure is based on CCI recommendations and is arranged as
follows:

`/cci_seastate/<release version>/data/<instrument type>/<type>/<mission>/<date>/`

Where:
* `<cci_project>` : *seastate*
* `<release version>` is the dataset version (currently 4.0 for this dataset)
* `<instrument type>` is the type of remote sensing technique: *altimeter* 
  or *sar*
* `<type>` : will be different for each ECV, but needs to be defined, and  
  consistent within an ECV, here l2 for along-track data, l3 for edited 
  merged  products and l4 for monthly averaged gridded products)
* `<mission>` : satellite mission (for L2P products only)
* `<date>` : *<year as YYYY>/<day in the year as DDD>*


## Data subsetting

Remote data subsetting and reading is also possible:
- with THREDDS/OPenDAP service: https://tds0.ifremer.fr/thredds/catalogs/ESACCI-SEASTATE-L4-SWH-MULTI_1M.html (L4 only) - check [this tutorial](./notebooks/tutorial_thredds.ipynb)
- with the ESA CCI Sea State API: https://cci-seastate.ifremer.fr/ (all 
  products) - check [this tutorial](./notebooks/tutorial_cci-seastate_subsetting_api.ipynb)



