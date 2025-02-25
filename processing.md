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

