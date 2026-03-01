# Gromacs_MD_simulation_protein
Scripts for simulating a single protein on GROMACS
Keep your PDB file, .mdp files and the required force field folders ready in a separate folder. Now open GROMACS in this folder by typing gmx on the terminal. 
Now you can start executing these scripts one by one. Choose 
Note: Please add salt ions and concentration as per your experimental needs. Do not ignore any warnings and use --maxwarn flag only if the warning is not harmful and does not change your MD simulation settings. Happy simulating!

gmx pdb2gmx -f your_protein.pdb -o processed.gro 


gmx editconf -f processed.gro -o newbox.gro -bt cubic -d 1.0


gmx solvate -cp newbox.gro -cs spc216.gro -p topol.top -o solv.gro


gmx grompp -f ions.mdp -c solv.gro -p topol.top -o ions.tpr


gmx genion -s ions.tpr -o solv_ions.gro -p topol.top -pname K -nname CL -conc 0.15 -neutral


gmx grompp -f minim.mdp -c solv_ions.gro -p topol.top -o em.tpr


gmx mdrun -v -deffnm em 


gmx grompp -f nvt.mdp -c em.gro -r em.gro -p topol.top -o nvt.tpr


gmx mdrun -v -deffnm nvt 


gmx grompp -f npt.mdp -c nvt.gro -t nvt.cpt -r nvt.gro -p topol.top -o npt.tpr


gmx mdrun -v -deffnm npt 


gmx grompp -f md.mdp -c npt.gro -t npt.cpt -p topol.top -o md.tpr


gmx mdrun -v -deffnm md
