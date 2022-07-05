# linescan-analysis

Script for the post-processing of line-scan measurements of vessels diameter oscillations.

## Usage

### Get the data

#### raw data (necessary for the peak to peak analysis)
Download the raw data archive : 160322_4traces.tar.gz 

Untar it in the data/raw folder

#### statistics data (necessary for the simulation preprocessing)

Download the statistics data archive : statistics_penetrating_arterioles_WT10.tar.gz 

Untar it in the data/statistics folder

#### simulation results data (necessary for the dispersion analysis)

Download the simulation results data archive : 
- dispersionRandomWT10t40area-d2e-07-l6e-02.tar.gz
- dispersionRandomWT10t40area-d7e-08-l6e-02.tar.gz
- intakeRandomWT10t200area-d2e-07-l6e-02.tar.gz
- intakeRandomWT10t200area-d7e-08-l6e-02.tar.gz

Untar them in the data/simulations folder


### Peak to peak analysis

The main script to generate the database containing the peak to peak features is 

/scripts/scanline-analysis.py

It is using the module defined in 

/src/datanalysis.py

It will create and save databases in the folder output/databases/

Figures showing the details of the analysis will be saved in the folder output/amp-analysis/

### Database conversion for statistical analysis

The database generated by  /scripts/scanline-analysis.py is converted into a cvs file, used for the statistical analysis. 
Before exportation, the data is cleaned by removing : nan values, outliers, etc.

The script for conversion is /scripts/convertcsv.py


### Statistical analysis 

A R script perfoms the statistical analysis. The output files consists in random analyses , were a model is fitted to each vessel. and a fixed effect analyses where a model is fitted to the whole population.

The results from this script is the satistics data, that can be douwnloaded and should be saved in the data/satistics folder.

### Preparation of simulations batch

From the statistical analysis we generate slurm file to lauchn corresponding simulation of CSF flow on the super computer. These slurm files are launched through a single batch file.

The script to generate the simulation batch files is : 
/supercomputer/batch_pythonscript.py

The slurmfiles will be stored in the folder output/supercomputer


### Simulations

The script to lauchn the simulation is scripts/PVS_simulation.py.

It must be called with several arguments that describe the PVS geometry and arteriole pulsations. The command lines can be viewed at the end of the slurm files generated in the previous stage.

For exemple a typical call for a simulations is 

python3 PVS_simulation.py -lpvs 0.02 -c0init gaussian -c0valueSAS 0 -c0valuePVS 1 -sasbc scenarioA -tend 40 -toutput 0.20053738530806994 -dt 0.0015666983227192964 -r -1 -nr 8 -nl 200 -d 1.68e-07 -s 0.0002 -xi 0.01 -j disp-d2e-07-l2e-02-baseline-card-v1e-02-id0-6-5 -ai 0.007979129036625841  -fi 9.973202736874004  -rv 0.0004984156211725926 -rpvs 0.0006981850068656416 -o '../output/simulations/'

You can look to the corresponding .log file in the output/simulations/ folder to check how the simulation is running.




### Dispersion analysis

The disperison analysis is performed on the simulations output files .log and .txt. 

Those files can be directly downloaded and saved in the folder data/simulations/.

It uses the concentration profiles at different times, fit a gaussian profile and deduce the appatent diffusion coeficient and the associated enhancement factor. To check the output one can look to the automatically generated reports (.txt) and images in the output folder. The script generate a database with the results which is saved in the output/dispersion/ folder.

The script for the Random effect dispersion analysis is :
/scripts/dispersion_analysis_gauss.py



### Post processing of results and figure creations

The script to generate figures from the dispersion analysis results is :

/scripts/dispersion_figure.py (Random effect)

and the transport figures are generated with the script : 

/scripts/transport_figures.py

