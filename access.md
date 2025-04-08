 # Data access

The data can be obtained from Ifremer FTP server: ftp://eftp.ifremer.fr. 

The login and password can be obtained upon filling the registration form at: https://forms.ifremer.fr/lops-siam/access-to-esa-cci-sea-state-data/

Latest data are also linked from the Project web page at: 
https://climate.esa.int/en/projects/sea-state/data/ .

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

