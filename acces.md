# Data access

The data can be obtained from Ifremer FTP server: ftp://eftp.ifremer.fr. The login and
password can be obtained upon filling the registration form at: https://forms.ifremer.fr/lops-siam/access-to-esa-cci-sea-state-data/
Latest data are also linked from the Project web page at: https://climate.esa.int/en/projects/sea-state/data/ .
The common directory structure is based on CCI recommendations and is arranged as
follows:

/products/<release version>/data/<instrument type>/<type>/<mission>/<date>/

Where:
* <cci_project> : seastate
* <release version> is the dataset version (currently 2.0.6 for this dataset)
* <instrument type> is the type of remote sensing technique: altimeter or sar
* <type> : will be different for each ECV, but needs to be defined, and consistent within an ECV, here l2 for along-track data, l3 for edited merged products and l4 for monthly averaged gridded products)
* <mission> : satellite mission (for L2P products only)
* <date> : <year as YYYY>/<day in the year as DDD>
The file nomenclature is based on form 2 of CCI recommendations :
ESACCI-<CCI Project>-<Processing Level>-<Data Type>-<Product String>[-<Additional Segregator>]-<IndicativeDate>[<Indicative Time>]-fv<File version>.nc

Where:
* <CCI Project> : SEASTATE
* <Processing Level> : here L2P for along-track data, L3 for edited merged product
and L4 for monthly averages
* <Data Type> : SWH for “Significant Wave Height”, ISSP for “Integrated Sea State Parameters”
* <Product String> : contains the name of the mission for L2P
* <Additional Segregator> : optionally the name of algorithm used
* <Indicative Date>[<Indicative Time>] : The identifying date for this data set. Format
is YYYYMMDDTHHMMSS, where YYYY is the four digit year, MM is the two digit
month from 01 to 12 and DD is the two digit day of the month from 01 to 31. The date
used should best represent the observation date for the data set. For along-track
data it will be the time of the first measurement in the file.
* <File version> : File version number in the form n{1,}[.n{1,}] (That is 1 or more digits
followed by optional . and another 1 or more digits.)


Examples:
L2P altimeter product:

ESACCI-SEASTATE-L2P-SWH-Jason-2-20170130T145103-fv01.nc
L3 product:
ESACCI-SEASTATE-L3-SWH-MULTI_1D-20170130-fv01.nc
L4 product:
ESACCI-SEASTATE-L4-SWH-MULTI_1M-201701-fv01.nc


