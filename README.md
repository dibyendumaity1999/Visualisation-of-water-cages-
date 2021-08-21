# Visualisation-of-water-cages-
Using VMD, GRADE and python , visualizing  water cages formed during simulation of methane hydrate

# Need an gromacs trajectory at first( or an snapshot!)
lets say you have traj.xtc compressed trajectory. Consider having a tpr(binary file produced by grompp) file because we need that during a gmx trjconv , i.e.,
gmx trjconv -f traj.xtc -s traj.tpr -o traj_full.gro
(traj_full contains all the frames ,in gro format)
# GRADE
GRADE analyzes atomic positions of oxygen atoms of water to compute the number of 5(12), 6(2)5(12) and 6(4)5(12) cages and account for their three-dimensional structures. The latter can be used for visualization using software such as VMD (Visual Molecular Dynamics). GRADE stands for “cages” in Portuguese. F4 order parameter can also be calculated for trajectories.
Further information can be found from : https://github.com/farbod-nobar/GRADE
To use GRADE , we execute the command like , 
./GRADE -i traj_full.gro -f4 yes 
As simple as that! You can use multiple arguments , like specifying solute molecules etc, but in general not required. Thing to consider , by default GRADE is for water cages so residue names for solvent requires to be "SOL" . That can be done easily!
Use sed for reference... for e.g., if your gro file has solvent residue ICE (generally comes when use genice to produce clatharate) then you can simply execute,
sed -i "s/ICE/SOL/gI" traj_full.gro 
It will work! Or you can go with any file editing methods , doesn't matter
At the end of the day you might have the outputs: F4.xvg, traj_full.xvg , and traj_full_cages_512_xx.gro,traj_full_cages_62512_xx.gro,traj_full_cages_64512_xx.gro for each frame. They contains the cage molecule information.
# VMD 
Now once you have gro file for the cages , you might project them in VMD! But hold on. You now have the requirment of gro file of each frames. What you have now is the full trajectory (traj_full.gro). Extracting frames from xtc file is piece of butter actually! You have to execute--
gmx trjconv -sep -f traj.xtc -s traj.tpr -o xxx.gro
It will create xxx0.gro xxx1.gro upto total number of frames. You can extract certaint frames using -fr option and so on... Just google arround if you don't know , it's that easy.
Now giving a tcl script which actually does the job. I mean come'on we don't want to do them'all manually right!
"Model.tcl"
And a python script which call's vmd terminal like and changes the tcl file to generate snapshots of each frame.
