# TD-DFT calculations in CP2K

## 1. CP2K input for electronic structure calculations

`cp2k_input_template.inp`

This file is a template of a cp2k input file and is used to compute the electronic structure of the system along the precomputed nuclear trajectory. SCF calculations are called with the parameter `RUN_TYPE ENERGY`. The input should be able to compute the TD-DFT calculations, producing the cube files and PDOS files.

In the section FORCE_EVAL < PROPERTIES is the input needed to compute TD-DFT calculations. One would also need to set completion_level = 0 in the file submit_template.slm.  

If the user needs to change the cp2k input, the changes should be done to this input file. One must make sure that the cube files can be produced via WRITE_CUBE .TRUE. 

Other required files for running the CP2K input file are basis set and pseudopotential files or any other files required to run the calculations, such as `dftd3.dat`. The full path to these files in the `cp2k_input_template.inp` file should be specified.

[CP2K paper](https://aip.scitation.org/doi/pdf/10.1063/5.0007045)

[Difference between TD-DFT and TD-DFPT](https://groups.google.com/g/cp2k/c/xj8udnSyeEI)


## 3. Run all the jobs through `run.py`

The required inputs in the `run.py` file are as follows:

`trajectory_xyz_file`: The user must specify the full path of the trajectory `xyz` file or the name of the file if it is in the current folder. 

`es_software_input_template`: The input template for the electronic structure calculations. Here, the input template is `cp2k_input_template.inp` file.

`es_software`: The name of the electronic structure calculations software e.g. `cp2k`.

`istep`: The initial step from the _trajectory `xyz` file_. 

`fstep`: The final step from the _trajectory `xyz` file_. 

`njobs`: The total number of jobs that the user wants to submit. The maximum number of jobs must be less than half of the total steps i.e. `fstep-istep+1`.

`os.system("sbatch submit_"+str(njob)+".slm")`: The jobs are submitted through this line of code at the end of the `run.py`. Please, change it according to your HPC submission platform. For example if you use `pbs` files and you use `qsub`, after preparing the `submit_template.pbs` the same as `submit_template.slm` you can change this line to `os.system("qsub submit_"+str(njob)+".pbs")`.

**_Note_:** you can read this [READMI](https://github.com/MohammadShakiba/Project_Libra_CP2K/tree/master/Cd33Se33/step2) for have more and complite information.

