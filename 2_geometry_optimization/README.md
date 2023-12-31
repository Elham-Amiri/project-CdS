# Geometry optimization

 We first need to obtain an optimized geometry of the structure. When we make a new structure, like heterostructures, we may also need to obtain an optimum cell as well. The cell optimization has only one more variable than geometry optimization: pressure and the optimizer will let the cell change its size after each optimization step. We have the cell optimization inputs in the ``` cell_optimization ``` folder.
 
 For other structures, like supercells, usually obtained from ``` cif files ``` the structure may be initially optimized when geometry optimization is performed but usually this is not the case because the energy calculations in each software package may be different. Therefore, you need to perform geometry optimization first. If the structure changes are massive, you need to move to cell optimization as well. You can first use an initial cutoff value for this purpose, which is obtained through the convergence analysis for the initial structure, and then after obtaining the optimized geometry one can perform the convergence analysis again to obtain another cutoff value if the initial cutoff is not the same as the one obtained from convergence analysis you can redo the geometry optimization by the new cutoff value.

 Note that the CP2K optimizer works based on the forces for each of geometry and cell optimization. In fact, the movement of the atoms is based on their computed forces. The lower the convergence criteria of the SCF cycle ``` (EPS_SCF) ``` the more accurate the computed forces are. This will lead to a better-optimized geometry and will not confuse the optimizer. 
 After you obtained the optimized geometry, for the MD you can use lower ``` EPS_SCF ``` values like ``` 10E-6.0 ```. But the more accurate the forces in MD the more accurate the time-overlaps are and therefore the more accurate the nonadiabatic couplings will be. This is up to the user on which ``` EPS_SCF ``` value to choose and is totally dependent on the studied system and its computational cost.
 
 The target forces and displacement for the geometry or cell optimizations are defined in the ``` &MOTION ``` and ``` &GEO_OPT ``` section with ``` MAX_FORCE ``` and ``` MAX_DR ``` keywords respectively. The force value unit is in ``` Hartree/Bohr ``` and the displacement unit is in Bohr. Here, the maximum force is set to 0.0003 Hartree/Bohr which is almost equivalent to 15 meV/A and the maximum displacement is set to 0.002 Bohr. When running the geometry optimization, it will print out the coordinates and forces in each step in ``` *_geo_opt-pos.xyz ``` and ``` *_geo_opt-frc.xyz ``` files. The last coordinates in the ``` *_geo_opt-pos.xyz ``` will be the optimized geometry.
 
 Also, ``` *.restart ``` files are produced that contain the information of the geometry optimization of the last step, and if the run is suddenly interrupted, you can change the extension to ``` .inp ``` by ``` mv ``` command and then run it again. This will continue the geometry optimization from the point it was interrupted.





