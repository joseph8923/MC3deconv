# MC3 deconvolution (ver. 0.0)
This package provides a open code to perform deconvolution of teleseismic waveforms (i.e., calculation of receiver-functions) via multichannel deconvolution by rj-McMC (MC3-deconvolution). 

---

## How to install

Compiling soce codes can be done by simiply typing, `make` in the main directory. This process assumes that `mpif90` is available and that the `mpif90` links to GNU frotran compiler (i.e., `gfortran`). Alternatively, you can use `mpif90` that links to Intel compiler (i.e., `ifort`). In this case, some modification is necessary in Makefile (see Makefile for details). Currently, we have chekced the program working correctly on centOS7 with GNU fortran compiler.


## How to run

Run the program by `mpirun -np (# of processes) bin/mc3deconv`. Since one of the processes are used only to control temperature swapping between McMC chains, at least two processes are needed. Also, a file, "params.in", must be exist in the current direcory, in which user-given parameters are to be specified. The easiest way to test is moving to "sample1" directory and running the program. All necessary input files are already there. 

`cd sample1`

`mpirun -np 2 ../bin/mc3deconv`

---

## Input

The program, `mc3deconv`, assumes a parameter file, in which tuning parameters and input data are specified, exists in the currenct directory with the name "params.in". The format of the parameter file is as below, but you can put comment lines that must star with #.

|Line #|parameter 1|parameter 2|Example value1| Example value2|
|:--:|:--:|:--:|:--:|:--|
|1| # of iterations in burn-in period|-|200000|-|
|2| # of iterations in sampling period|-|1200000|-|
|3| # of iterations per generating one sample|-|200|-|
|4| Your favorate number (used to initialize pseudo random numbers)||21120903|-|
|5| # of McMC chains per process (for parallel tempering|-|10|-|
|6| # of non-tempered chains|-|2|-|
|7| Maximum temperature|-|100.0|-|
|8| Input Z component file (in SAC format) | -|input/syn_obs.z|-|
|9| Input R component file (in SAC format)|-|input/syn_obs.r|-|
|10| Sampling interval of input data (sec)|-|0.05|-|
|11|Start time of the analysis window relative to file beginning (s)| End time (s) |195.0|255.0|
|12| Lower prior limit for # of pulses | Upper limit|1|101|
|13| Lower prior limit for Z amplitude | Upper limit|-0.8|0.8|
|14| Lower prior limit for R amplitude | Upper limit|-0.8|0.8|
|15| Lower piror limit for pulse timing relative to P (s) | Upper limit|1.0|60.0|
|16| Probability of birth proposal (adding a pulse) |-|0.35|| 
|17| Probability of death proposal (removing a pulse)|-|0.35||
|18| Probability of time-shit proposal |-|0.05|-|
|19| Probability of amplitude-perturb proposal |-|0.25|-|
|20| Standard deviation to perturb Z amplitude | Same but for R amplitude| 0.05 | 0.05|
|21| Standard deviation to newly generate Z amplitude | Same but for R amplitude | 0.4 | 0.4|
|22| Standard deviation to shift timing (s) | -|0.1|-|
|23| Total time length of output (s) | -|63.0|-|
|24| Acausal time length preceding P in output (s) | - | 3.0 | -|
|25| Factor of Gaussian low-pass filter | - | 8.0 | -|
|26| Minimum amplitudes for output| Maximum | -2.2 | 2.2|
|27| Amplitude bin width for output| - | 0.02 | |


---

## Output

In the current version (ver. 0.0), five output files are created after running the program.

### dim.ppd (ascii format)

This file contains marginal probability function of the number of pulses. The format is:

|1st column|2nd column|
|:--:|:--:|
|# of pulses|probability|


### Gr.ppd / Gz.ppd (ascii forat)

These files includes the posterior probability distribution of R and Z component Green's functions. The format is:

|1st column|2nd column|3rd colmun|
|:--:|:--:|:--:|
|time after P (s)|amplitude|probability|`

### Gr.mean / Gz.mean (SAC format)

These files includes the mean models of R and Z component Green's functions in [SAC](http://ds.iris.edu/files/sac-manual/) format.

---

## Samples
### Sample1: Synthetic data
### Sample2: Real OBS data
