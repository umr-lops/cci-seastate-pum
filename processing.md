# Processing procedures

### I. Retracking <a name="retracking"></a>

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
python /path/to/launcher/whales -i $1 -m jason1 -o /path/to/output/files
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


### II. Average to 1Hz <a name="average"></a>

**Step 1:** Git Environment

Before proceeding with this part of the process, ensure that you have all the necessary Git modules installed in your working environment.
These modules are essential for performing the data compression :
- cciseastate : https://gitlab.ifremer.fr/cciseastate/cciseastate
- ceraux : https://gitlab.ifremer.fr/cerbere/ceraux
- cerberecontrib-altimeter : https://gitlab.ifremer.fr/cerbere/cerbercontrib-altimeter
- cerberecontrib-whales : https://gitlab.ifremer.fr/cerbere/cerberecontrib-whales
- cerbere : https://gitlab.ifremer.fr/cerbere/ceraux
- ceremd : https://gitlab.ifremer.fr/cerbere/ceremd
- cerform : https://gitlab.ifremer.fr/cerbere/cerform

Ensure you clone or update each of these modules in your working environment : 
```bash
#Exemple command to clone one of the modules
git clone https://gitlab.ifremer.fr/cerbere/cerform
```


