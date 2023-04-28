Overview

1.Introduction
2.Repo Contents
3.System Requirements
4.Installation Guide
5.Demo
6.Results



1.Introduction
The repository contains the codes and script for simulating 3D chromosome structures from DNA accessibility data based on stochastic folding model. The code is written by C and the script is written in bash
Both the source code and the compiled code are presented in the repository. 





2.Repo Contents
prepare: repository to convert DNA accessibility data to the input data prepared for simulating 3D chromatin structures 
simulation: repository for calculating folded chromatin structures in confined space based on stochastic folding.
analysis: repository to identify domain boundary from simulated chromatin structure ensemble.

In the respository ./prepare/data_raw/, there are two files obtained from experimental data: 
ATAC_data.dat: DNA accessibility data presented in four column, ie chrid, start_pos, end_pos, signal value. Here we use ATAC-seq data as an example
hg19.chrom.sizes: file containing the length of reference genome.


The calculation can be roughly divided into 3 steps:
(1) preparation step. In this step, we construct 10 polymer models from DNA accessibility data and generate low-resolution initial structures as starting points for structural simulation
command: bash prepareall.sh
(2) simulation step: In this step, we first simulate the chromatin structures of low-resolution model based on stochastic folding and then convert the simulated low-resolution structures to high-resolution ones. We repeat this simulation and structural convert until 10-kb resolution structures are achieved
command: bash do_simulation.sh
(3) structure analysis: In this step, we first convert the files containing the coordinates of each polymer beads to the files containing the coordinates of each 10-kb segments. With the converted files, we then calculate the separation score for each 10-kb segments, based on which we can identify the genomic positions of domain boundaries.
command: bash do_analysis.sh



3.System Requirements
The code is tested on Linux operating systems with the system Ubuntu 16.04
It requires only a standard computer with enough RAM to support the calculation. We recommend a computer with the following specs:

RAM: 8+ GB
CPU: 4+ cores, 3.2+ GHz/core



4.Installation Guide
awk and mysql are required before running the script, which can be installed by 

sudo apt-get install mawk
sudo apt-get install mysql-server

it may take serveral minutes to finish the installation



5.Demo
Just run the three scripts one by one
bash prepareall.sh	#this step is fast and it may take several minutes to finish
bash do_simulation.sh	#this script is designed using one core and it may take several months to finish this. you can modify the script to use multiple cores to improve the efficiency
bash do_analysis.sh	#it may take several hours to finish



6.Results
After finishing these 3 steps, the files containing coordinates of simulated chromatin regions are deposed in the directory ./analysis/conf/mergepoint/ with filename as 1.dat, 2.dat,... In these coordinate files, each row corresponds to one 10-kb segment and these 10-kb segments are listed in the sequential order. The chromosome id of each row is listed in the file ./prepare/assign/detail/chrassign_10kb.dat
