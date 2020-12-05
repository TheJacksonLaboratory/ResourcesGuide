---
title: "Working with the hpc cluster"
author: "Carter Lab"
date: "2020-12-05"
output:
  html_document:
    toc: yes
    toc_float: yes
    theme: cerulean
    highlight: textmate
---



Two ways to work with the hpcc: interactive sessions and job submission. Both need to be done using singularity and containers. It's a good idea to set up the hpc environment with some "module-like" infrastructure to allow some flexibility in interactive sessions for development and testing. Otherwise need to either pull or build a container that has every needed package/software in it for every project.
For a comparison of using a module-like set up vs containers see [here](https://hpctalk.jax.org/t/need-software-that-isn-t-installed-on-sumner/71)  

## Setting up your hpc environment
Open a terminal app (either stand-alone or RStudio).  
Tip: If you use the terminal tab in RStudio. You can then source code directly from your editor to the terminal with source code by pressing alt-cmd-enter when standing on the code line you want to source.

1. Log into sumner  

```bash
ssh USERNAME@login.sumner.jax.org
```

2. Create a directory for all your containers, packages, etc    

```bash
mkdir bin
```

3. Add bin into your $PATH  
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Executing-Host-Machine-Binaries-Inside-a-Container.aspx  
Open .bash_profile with a text editor

```bash
cd ~ # make sure you're in home dir
nano .bash_profile # using nano to edit the file
```
And add these lines to the file

```bash
#Singularity Containers
export PATH=$PATH:~/bin # change the path accordingly if you want to put the bin elsewhere
```
Save and exit the editor, and restart your terminal.  
Now you can call containers by their name from anywhere in your home directory, without having to specify the path.

4. Initialize an interactive session, load singularity, and pull containers:  
Here we pull the latest r container and put it in the bin directory.  
For easy reference we rename it R.sif  
https://singularity-hub.org/collections/4747/usage  

```bash
srun --pty -q batch bash # to start an interactive session
module load singularity
cd ~/bin
singularity pull --name R.sif docker://r-base:latest
# singularity pull --name R_tidy.sif docker://rocker/tidyverse:latest
# singularity pull docker://bioconductor/bioconductor_docker:devel
```
Now you can start an interactive session with R (see below) because a sif file can be used as an executable file on its own. 
You can do the same with any software, e.g., samtools from jaxreg

**Close the interactive session by typing ```exit``` or ```ctrl-D```**  

5. Add jax container registry as a remote endpoint:  
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/JAX-Singularity-Container-Registry-User-Guide.aspx  

**Type ```logout``` to close the connection to sumner**  
See also https://hpctalk.jax.org/t/sumner-crash-course/77  

## Installing software on hpc
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Installing-Software-on-the-Cluster.aspx  
https://hpctalk.jax.org/t/need-software-that-isn-t-installed-on-sumner/71  

## Working interactively on the cluster
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Interactive-Jobs-with-SLURM.aspx  
Working interactively has the potential to drain the hpc resources for everyone else. Therefore, please keep in mind the following:  
1. Shut down the session when you're done working by typing ```exit``` or ```ctrl-D```. Idle sessions are killed pretty quickly.  
2. Don't over-allocate resources when initializing the session. Over-allocation will get your session killed and your rapport with IT ruined.  
3. Be prepared to lose your work at any time. Interactive sessions will be the first to go in times of high demand. Script everything.  

See ```man srun``` for options such as CPU and memory allocation  

### From your local RStudio app
Open a new terminal in RStudio. Once logged into sumner enter this to start an interactive session with R:  

```bash
srun --pty -q batch bash
module load singularity
R.sif
```
You can now install R packages to your home directory to be available for future sessions, (almost) like you would on your local machine (or in the past, on helix).  

Once you're in R:

```r
# Specify libs before installing anything
dir.create("~/bin/libs", recursive=T)
.libPaths("~/bin/libs") # once every session

# For example:
install.packages("here", dependencies = TRUE) # only once
library(here) # once every session
here()

install.packages("tidyverse", dependencies = TRUE) # need to install some dependencies manually FIND CONTAINER TO REPLACE R.sif
```
tip: If you use the terminal tab in RStudio. You can then source code directly from your editor to the terminal with source code by pressing alt-cmd-enter when standing on the code line you want to source.  

Type ```q()``` to quite R. You're still connected to the interactive session.  

**Note:**  
You can do the above from any tier1 folder.  
If you want to work on a project in e.g., ```/sdata/```, you can navigate to it at any time: before intializing the interactive session, before calling R.sif, or from inside R.  

This option is great for installing packages and doing simple tests on hpc while developing your project mostly on your local machine. You will need to synchronize your local project folder with a copy on tier1 (see section 5 [Organizing projects] below).
However, in order to enjoy the full functionality of RStudio (and the Rproj setup) while working directly on your tier1 copy see the next section.  
If you're developing the project mostly with RStudio, it would probably be easier to move your project to tier2 and use the [RStudio Server].

### Remote RStudio

You can [run Rstudio Server on the hpc](https://divingintogeneticsandgenomics.rbind.io/post/run-rstudio-server-with-singularity-on-hpc/) and access it from a local browser via port 8787.  
You'll need to install it with singularity from [the rocker project](https://www.rocker-project.org/use/singularity/).  
Here logging out of a session is crucial not only to avoid draining hpc resources but also to avoid RStudio session log filling up your home storage.  

Two other options are the R packages [remoter](https://cran.r-project.org/web/packages/remoter/index.html) and [rmote](https://github.com/cloudyr/rmote), but on the face of it they don't seem very helpful for most of our needs. Package rmote uses the cluster for computations while using local web browser for resulting graphics, but is not available for newer versions of R and doesn't seem to be actively maintained anymore. Package remoter is part of [a big and actively maintained project](https://pbdr.org/index.html), and allows running remote sessions in parallel to local ones, including "remote" sessions on you local machine, but does not provide graphic interface and visualization for those sessions.


## Submitting jobs to the cluster
https://slurm.schedmd.com/rosetta.pdf
https://slurm.schedmd.com/quickstart.html


