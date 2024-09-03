
# Installation

The following sections describes how to install and deploy the CCI Sea State 
processing system.

```bash
mamba create -n cciseastate python=3.10
mamba activate cciseastate
```

## Install processors

```bash
mamba install dask xarray  shapely scipy netCDF4 PyYAML pydantic git-lfs

git clone https://gitlab.ifremer.fr/cerbere/cerbere.git
pip install cerbere/

git clone https://gitlab.ifremer.fr/cerbere/cerberecontrib-whales
pip install cerberecontrib-whales/

git clone https://gitlab.ifremer.fr/cerbere/cerbercontrib-altimeter.git
pip install cerbercontrib-altimeter/

mamba install gdal
git clone https://gitlab.ifremer.fr/cerbere/ceraux.git@cerbere3
pip install ceraux/

mamba install Cython pyresample
git clone https://gitlab.ifremer.fr/cerbere/cerinterp.git
pip install cerinterp/

mamba install statsmodels
git clone https://gitlab.ifremer.fr/ceremd/ceremd.git
pip install ceremd/

git clone https://gitlab.ifremer.fr/cciseastate/cciseastate.git
pip install cciseastate/
```

## Install documentation

```bash
mamba install jupyter-book
git clone https://gitlab.ifremer.fr/cciseastate/ccidoc.git
```

