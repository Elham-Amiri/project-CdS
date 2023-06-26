# TD-DFT calculations in CP2K

## 1. CP2K input for electronic structure calculations

`cp2k_input_template.inp`

This file is a template of a cp2k input file and is used to compute the electronic structure of the system along the precomputed nuclear trajectory. SCF calculations are called with the parameter `RUN_TYPE ENERGY`. The input should be able to compute the TD-DFT calculations, producing the cube files and PDOS files.

In the section FORCE_EVAL < PROPERTIES, is the input needed to compute TD-DFT calculations. One would also need to set completion_level = 0 in the file submit_template.slm.  

If the user needs to change the cp2k input, the changes should be done to this input file. One must make sure that the cube files can be produced via WRITE_CUBE .TRUE. 

Other required files for running the CP2K input file are basis set and pseudopotential files or any other files required to run the calculations, such as `dftd3.dat`. The full path to these files in the `cp2k_input_template.inp` file shoud be specified.

[CP2K paper](https://aip.scitation.org/doi/pdf/10.1063/5.0007045)

[Difference between TD-DFT and TD-DFPT](https://groups.google.com/g/cp2k/c/xj8udnSyeEI)

The keyword `RESTART` increases the speed of calculations, both for SCF and TD-DFPT calculations. Therefore, the `RESTART` is required to be set to `.TRUE.` in `TDDFPT` section. Also, the `WFN_RESTART_FILE_NAME` in this section should exist with a random `tdwfn` file name. The same is also needed in the `FORCE_EVAL` section. `WFN_RESTRAT_FILE_NAME` should exist in the input with an random `wfn` file name. 

In the `&MO_CUBES` section the number of occupied and unoccupied orbitals must be specified. This is dependent on the TD-DFPT calculations and the user have to make a good guess to make sure that the cube files of all the states in the excitation analysis of TD-DFPT calculations exist. This guess can be obtained from running the calculations for 5-10 steps.

## 2. Bash file for running the calculations for one job

The standard sample bash file for submitting the calculations and running the Python code through `slurm` and `sbatch` is the `submit_template.slm`. First one should load all needed modules including the modules required for loading CP2K. This file contains the input variables required for calculations of the overlap matrices and NACs. We list the variables as follows:

`nprocs`: The number of processors used for calculations. Note that the same number of processors should be specified in the `#SBATCH --ntasks-per-node` above.

`cp2k_exe`: The executable CP2K or the full path to executable CP2K folder.

`res`: The directory for storing the overlap matrices and NACs.

`min_band`: The minimum KS orbital index to be considered.

`max_band`: The maximum KS orbital index to be considered.

`ks_orbital_homo_index`: The HOMO index for KS orbitals.

