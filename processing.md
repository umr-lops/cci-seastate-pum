# Processing procedures

## Retracking

**Step 1 :** Retrieving SGDR Files

The first step in this process is to retrieve the SGDR files for retracking them. To do this, you must make sure you have de WHALES retracker in your working environment.
The procedure can be found here : https://gitlab.ifremer.fr/cciseastate/whales

Then, create a file with a .list extension, which will contain the list of SGDR files to be retracked.
Each line of the file should specify the full path to a SGDR file.

**Step 2 :** Using a production space

Before starting the script execution, you need to specify a production space to store the files generated after processing. This directory should be dedicated to the output of the retracked files.
Another space should be dedicated to storing .list files and scripts.

**Step 3 :** Creating the processing script

A Bash script should be set up to automate the retracking process using the WHALES module.
Here is an exemple of a script you can use :

```bash
#!/usr/bin/env bash
#PBS -l walltime=00:35:00
#PBS -l mem=5g

#Loading the necessary modules
source /usr/share/Modules/3.2.10/init/bash
module load anaconda-py2.7/4.3.13

#Activating the specific conda environment
source activate /path/to/env/conda

#Displaying the working directory
echo $PBS_O_WORKDIR

#Navigating to the WHALES directory
cd /path/to/module/whales

#Running the WHALES retracker script with parameters
python /path/to/launcher/whales -i $1 -m jason1 -o /path/to/output/directory
```

Script options : 
- -i : specifies the input file
- -m : defines the mission
- -o : indicates the output directory where the retracked files will be stored

**Step 4 :** Running the execution with prun

To automate the execution of the process for multiple SGDR files, use the prun command with the .list file :
```bash
cat fichier.list | /appli/services/bin/prun -e /path/to/script --split-max-jobs=100 --max-time 23:00:00
```

Once the process is complete, the generated files will be stored in the production space specified in the scripts -o option.

Paths to SGDR data to put as input in the script are listed in 
{numref}`sgdr_inputs`.

## Average to 1Hz

**Step 1:** Git Environment

Before proceeding with this part of the process, ensure that you have all the necessary Git modules installed in your working environment. (See the Installation chapter).

Ensure you clone or update each of these modules in your working environment : 
```bash
#Exemple command to clone one of the modules
git clone https://gitlab.ifremer.fr/cerbere/cerform
```

**Step 2 :** Using a production space

Before starting the script execution, you need to specify a production space to store the files generated after processing. This directory should be dedicated to the output of the retracked files.
Another space should be dedicated to storing .list files and scripts.

**Step 3 :** Creating the processing script

A Bash script should be set up to automate the averaging to 1Hz process.
Here is an exemple of a script you can use :

```bash
#!/usr/bin/env bash


cd $PBS_O_WORKDIR

#Loading the necessary modules
source /usr/share/Modules/3.2.10/init/bash
module load anaconda-py2.7/4.3.13

#Activating the specific conda environment
source activate /path/to/env/conda

#Running the average_to_1hz script with parameters
average_to_1hz -f $1 -m mission -c /path/to/configuration/file -o /path/to/output/directory

```

Script options : 
- -f : specifies the input file
- -m : defines the mission
- -c : indicates the path to the configuration file
- -o : indicates the output directory where the 1Hz files will be stored

Paths to retracked files to put as input in the script are listed in 
{numref}`retracked_workspace`.

## L2P

**Step 1 :** Using a production space

Same as for Average to 1hz step

**Step 2 :** Creating the processing script 

A Bash script should be set up to automate the L2P process.
Here is an exemple of a script you can use :

```bash
#!/usr/bin/env bash


cd $PBS_O_WORKDIR

#Loading the necessary modules
source /usr/share/Modules/3.2.10/init/bash
module load anaconda-py2.7/4.3.13

#Activating the specific conda environment
source activate /path/to/env/conda

#Running the average_to_1hz script with parameters
l2tol2p $1 mission /path/to/configuration/file -o /path/to/output/directory

```

Script options :
- -o : indicates the output directory where the L2P files will be stored

## Generating the reference in situ buoy data

The buoy data used as in situ reference measurements are collected from the
Copernicus Marine Service multi-year dataset, quality controlled and saved into 
monthly parquet files:

```bash
cmems_wave_data2felyx  '/path/to/Copernicus/data/INSITU_GLO_WAV_DISCRETE_MY_013_045/cmems_obs-ins_glo_wav_my_na_irr/history/MO/??_TS_MO_*.nc' cmems_wave -m  -f 1D --start 2022-01-01T00:00:00 --end 2023-01-01T00:00:00 --concatenate -o /path/to/output/directory/
```

## Generating match-ups with in situ buoys

The generation of the matchups between satellite and in situ buoys is processed 
with the *felyx* framework (https://felyx.gitlab-pages.ifremer.fr/felyx_docs/).

It is a two-step process, first extracting the matchups from every satellite 
file and then collating the matchups into multi-matchup periodic files.

To use felyx, the following conda environment must be used on *Datarmor* HPC 
infrastructure:

```bash
/path/to/conda/envs/conda-env/felyx-processor-2.5.7
```

Extracting the matchups from a single satellite file, can be done in one single
line command:

````bash
felyx-extraction -c /path/to/felyx/configuration/file/cciseastate_mmdb.yaml --dataset-id ESACCI-SEASTATE-L2P-SWH-Cryosat-2e --inputs /path/to/input/satellite/file --manifest-dir /path/to/output/manifest/directory/
````

It creates a manifest file containing the reference to each found matchup 
(location within the input file of the closest satellite measurement).

This operation can be distributed over multiple nodes, using the job-array 
orchestrator *prun* available on *Datarmor*:

```bash
cat cryosat2e_l2p.list | /appli/services/bin/prun -e /path/to/extraction/script --split-max-jobs=100 --max-time 23:00:00
```

where the extraction script is a wrap-up bash script around the ``felyx-extraction`` 
command:

```bash
#!/usr/bin/env bash
#PBS -l mem=5g
#PBS -l walltime=00:30:00

cd $PBS_O_WORKDIR

source /usr/share/Modules/3.2.10/init/bash
module load anaconda-py2.7/4.3.13
source activate /path/to/conda/envs/conda-env/felyx-processor-2.5.7

echo $PBS_O_WORKDIR

felyx-extraction -c /path/to/felyx/configuration/file/cciseastate_mmdb.yaml --dataset-id ESACCI-SEASTATE-L2P-SWH-Cryosat-2e --inputs $1 --manifest-dir /path/to/output/manifest/directory/
```

Once the matchups have been extracted, they can be assembled into monthly 
multi-matchup files, using ``felyx-assemble`` command:

```bash
felyx-assemble --matchup-dataset ESACCI-SEASTATE-L2P-SWH-Cryosat-2e__cmems_wave --configuration /path/to/felyx/configuration/file/cciseastate_mmdb.yaml --output-dir /path/to/output/matchup/directory/cryosat-2e/ --manifest-dir /path/to/output/manifest/directory/ --start $start --end $end --from-manifests --extract-from-source
```

Similarly, this operation can be distributed with `prun`, providing as input the
list of months to generate:

```bash
cat cryosat2e_dates.list | /appli/services/bin/prun -e /path/to/assembling/script --split-max-jobs=100 --max-time 23:00:00
```

where the assembling script is a wrap-up bash script around the ``felyx-assemble`` 
command:

```bash
#!/usr/bin/env bash
#PBS -l mem=30g
#PBS -l walltime=23:00:00

cd $PBS_O_WORKDIR

source /usr/share/Modules/3.2.10/init/bash
module load anaconda-py2.7/4.3.13
source activate /path/to/conda/envs/conda-env/felyx-processor-2.5.7


echo $PBS_O_WORKDIR

start=$(date -d "$1" +%Y-%m-%d)
end=$(date -d "$1+1 month" +%Y-%m-%d)

echo $start $end


felyx-assemble --matchup-dataset ESACCI-SEASTATE-L2P-SWH-Cryosat-2e__cmems_wave --configuration /path/to/felyx/configuration/file/cciseastate_mmdb.yaml --output-dir /path/to/output/matchup/directory/cryosat-2e/ --manifest-dir /path/to/output/manifest/directory/ --start $start --end $end --from-manifests --extract-from-source
```

## Generating satellite cross-overs

Extracting crossovers between pairs of satellite missions is done with *naiad* 
software framework. This process consists in multiple steps:
- extracting the spatial and temporal 

To use *naiad*, the following conda environment must be used on *Datarmor* HPC 
infrastructure:

```bash
/path/to/conda/envs/conda-env/naiad-4.0
```

The first step is to create the index files from the satellite data, extracting 
the spatial footprint and temporal extent of their data, using the `naiad-index`
command:

```bash
#!/usr/bin/env bash


cd $PBS_O_WORKDIR


source /usr/share/Modules/3.2.10/init/bash
module load anaconda-py2.7/4.3.13

source activate /path/to/conda/envs/conda-env/naiad

naiad-create-index-file $1 spherical --feature-class Trajectory  --resolution=0.07 --segment-steps "time:10" --index-segments --gap-threshold 1 --output-dir /path/to/index/directory/cryosat-2e/ --archive 
```

These index must be registered into Elasticsearch v7 search engine for faster 
queries. First create an index for the mission:

```bash
naiad-create-index --index cciseastate_l2p_alt_cryosat-2e --dims=time ES7 --login=<username> --password=<password> --url=<elasticsearch server URL> --prefix=isi_cersat_naiad_fix=<prefix>
```

then register the index files into Elasticsearch:

```bash
find /path/to/index/directory/cryosat-2e/ -maxdepth 1 -type d -wholename "*cryosat-2e/20*" | xargs -ifoo naiad-register -q --index cciseastate_l2p_alt_cryosat-2e --records "foo/*/*.idx" --bulk ES7  --login=<username> --password=<password> --url=<elasticsearch server URL> --prefix=isi_cersat_naiad_fix=<prefix>
```

Having done so, the index of each mission can be used to infer cross-overs:







