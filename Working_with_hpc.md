---
title: "Working with the hpc clusters"
author: "Carter Lab"
date: "2022-04-15"
---



Two ways to work with the hpcc: interactive sessions and job submission. Both need to be done using [singularity and containers](Containerizing.html). You might want to set up the hpc environment with some ["module-like" infrastructure](https://hpctalk.jax.org/t/need-software-that-isn-t-installed-on-sumner/71) to allow some flexibility in interactive sessions for development and testing.

Other useful references:  
[Jax RIT documentation](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Intro-to-HPC---Documentation.aspx)
[Introduction to HPC from the HPC Carpentry](https://hpc-carpentry.github.io/hpc-intro/)  
[Samir Amin's documentation](https://sumner.verhaaklab.com/)  
[Pawsey Center SC workshop](https://pawseysc.github.io/sc19-containers/)

## Setting up your hpc environment

1. Log into sumner from your terminal  

```bash
ssh <USERNAME>@login.sumner.jax.org
```

2. Create a directory for all your containers, packages, etc and add it to your $PATH  
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Executing-Host-Machine-Binaries-Inside-a-Container.aspx  

```bash
mkdir bin
```
Open .bash_profile with a text editor:

```bash
cd ~ # make sure you're in home dir
nano .bash_profile # using nano to edit the file
```
And add these lines to the file (including the comment):

```bash
#Singularity Containers
export PATH=$PATH:~/bin
```
Change the path accordingly if you want to put the bin elsewhere.  
Save and exit the editor, and restart your terminal.  
Now you can call containers by their name from anywhere in your home directory, without having to specify the path.

3. Initialize an interactive session, load singularity, and pull containers:  
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

**Close the interactive session by typing `exit` or `ctrl-D`**  

4. Add jax container registry as a remote endpoint:  
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/JAX-Singularity-Container-Registry-User-Guide.aspx  

**Type `logout` to close the connection to sumner**  
See also https://hpctalk.jax.org/t/sumner-crash-course/77  

See these links for installing software on the cluster:
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Installing-Software-on-the-Cluster.aspx  
https://hpctalk.jax.org/t/need-software-that-isn-t-installed-on-sumner/71  

## Working interactively on the cluster
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Interactive-Jobs-with-SLURM.aspx  
Working interactively has the potential to drain the hpc resources for everyone else. Therefore, please keep in mind the following:  
1. Shut down the session when you're done working by typing `exit` or `ctrl-D`. Idle sessions are killed pretty quickly.  
2. Don't over-allocate resources when initializing the session. Over-allocation will get your session killed and your rapport with IT tarnished.   
3. Be prepared to lose your work at any time. Interactive sessions will be the first to go in times of high demand. Script everything.  

See `man srun` for options such as CPU and memory allocation  

### From your local terminal/RStudio

This approach allows you to install packages and doing simple tests on the cluster while developing your project mostly on your local machine. You will need to synchronize your local project folder with a full copy on tier1.
However, in order to enjoy the full functionality of RStudio (and the R Project setup) while working directly on your tier1 folder see the next section.  
If you're developing the project mostly with RStudio, and don't need any files from tier1, it might be easier to move your project to tier2 and use the [RStudio Server](https://rstudio.jax.org/).
  
Once logged into sumner enter this to start an interactive session with R:  

```bash
srun --pty -q batch bash
module load singularity
R.sif
```
You can now install R packages to your home directory to be available for future sessions, (almost) like you would on your local machine (or in the past, on helix).  

**Tip:** If you use the terminal tab in RStudio, you can source code directly from your editor to the terminal by pressing ctrl-cmd-enter.

Once you're in R:

```r
# Specify libs before installing anything
# dir.create("~/bin/libs", recursive=T) # only the first time
.libPaths("~/bin/libs") # once every session

# For example:
install.packages("here", dependencies = TRUE) # only once
library(here) # once every session
here()
```
Type `q()` to quite R. You're still connected to the interactive session.  

**Note:**You can do the above from any tier1 folder. If you want to work on a project in e.g., `/sdata/`, you can navigate to it at any time: before initializing the interactive session, before calling R.sif, or from inside R.  

Installing packages interactively like this assumes that system dependencies of a given package already exist on the cluster or in the container, since us regular folks don't have root privileges to install these dependencies on the cluster. For example, if you try to install tidyverse in this manner, you'd get error messages saying that some packages such as libxml, libcurl, openssl, etc. are not found. Therefore, tidyverse and/or these system dependencies need to be included in the container from the get go. See the [Containerizing](Containerizing.html) section for more details.  
You can pull an image that already contains tidyverse (and RStudio etc) from [rocker](https://hub.docker.com/r/rocker/tidyverse), or you can pull a container from [jax registry](library://habera/rnaseq-modelad/rstudio_etc_4.1.3:1.0) that already contains bioconductor, renv, synapser, and other useful packages, in addition to tidyverse and RStudio. This second container has also been tested and works with remote RStudio as explained in the following section. If your project uses packages that need other system dependencies you'd need to add those to the def file of one of these containers and rebuild the sif file as explained [here](Containerizing.html).

```bash
singularity pull --name rstudio_etc_4.0.3.sif library://habera/rnaseq-modelad/rstudio_etc_4.0.3:1.0
singularity exec ~/bin/rstudio_etc_4.0.3.sif R
# singularity pull --name R_tidy.sif docker://rocker/tidyverse:latest
# singularity exec ~/bin/R_tidy.sif R
```
In this case we can't just open an R session by calling the sif file because of how its def file is designed, so instead we do it with `exec`.  
You can now load any of the pre-installed packages as usual.
  
**These R packages may also be useful for connecting to a remote server from your local environment:**
[SSH](https://cran.r-project.org/web/packages/ssh/)  
[remoter](https://cran.r-project.org/web/packages/remoter/)  
[rmote](https://github.com/cloudyr/rmote)  
[ssh-utils](https://cran.r-project.org/web/packages/ssh.utils/index.html)  


## Remote RStudio

You can run Rstudio Server on the hpc and access it from a local browser, as explained [here](https://pawseysc.github.io/sc19-containers/08-gui-rstudio/index.html) and [here](https://divingintogeneticsandgenomics.rbind.io/post/run-rstudio-server-with-singularity-on-hpc/).

Here's a similar solution put together by [Bill Flynn](https://thejacksonlaboratory.slack.com/archives/CMKS61RLM/p1610466642060300?thread_ts=1610466397.059200&cid=CMKS61RLM) (and slightly modified here):  
  
Open a text editor and create a file named `launch_rstudio.sbatch` with the following content. You might want to use your home directory instead of fastscratch.
```
#!/usr/bin/env bash
### SLURM HEADER
#SBATCH --output=/home/<USERNAME>/logs/rstudio-%j.log  #!!
#SBATCH --mail-type=FAIL
#SBATCH --mail-user=<FIRST.LAST>@jax.org #!!
#SBATCH --qos=batch
#SBATCH --time=72:00:00
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=2
#SBATCH --mem=64GB
#SBATCH --export=ALL
### SLURM HEADER
localcores=${SLURM_CPUS_PER_TASK}
simg_path="/home/<USERNAME>/bin/rstudio_etc_4.0.3.sif" #!!
work_dir="/home/<USERNAME>/work" #!!
set -euo pipefail
#mkdir -p ${work_dir}
cd ${work_dir}
mkdir -p run tmp
export PASSWORD=$(openssl rand -base64 15)
PORT=$(shuf -i8899-11999 -n1)
hostname_with_port=$(echo $(hostname -A):${PORT} | tr -d " ")
echo Login to ${hostname_with_port} with username: ${USER} password: ${PASSWORD}
module load singularity
singularity exec \
    -B $(pwd)/run:/run \
    -B $(pwd)/tmp:/tmp \
	-B /sdata/carter-lab \
    ${simg_path} rserver \
    --www-port ${PORT} \
    --auth-none=0 --auth-pam-helper-path=pam-helper \
    --auth-timeout-minutes=0 \
    --auth-stay-signed-in-days=30

```
Change every line that ends with `#!!` to reflect your own info. Save it to your home directory.  
Open an interactive session on sumner

```bash
srun --pty -q batch bash
module load singularity
```
Pull rstudio image from [jax registry](https://jaxreg.jax.org/containers/447), put it in your `simg_path`, and launch it:   

```bash
singularity pull --name rstudio_etc_4.0.3.sif library://habera/rnaseq-modelad/rstudio_etc_4.0.3:1.0
sbatch launch_rstudio.sbatch
```
You can also download it directly from the [jax registry](https://jaxreg.jax.org/containers/447).

Next you need to get the URL and login information from the log file: 

```bash
head -n 3 /home/<USERNAME>/logs/rstudio-<job_id>.log
```
You'll see something like this:
```
Login to sumnerXYZ.sumner.jax.org:12345 with
  username: ....
  password: ....
```
Go to that URL and enter these username and password.

**Type `scancel <job_id>` in your terminal when you're ready to end the session**  
Ending the session is crucial here not only to avoid draining hpc resources but also to avoid RStudio session log filling up your allotted storage.

**Note:**  
- Make sure all the directories in the launch file exist before running it.   
- If you already have the image in the relevant directory then you can just run the sbatch command from the login node. You only need to start an interactive job in order to pull the image.  
- You might want to use [`renv`](https://rstudio.github.io/renv/articles/docker.html) to [manage your locally-installed packages](Organizing_projects.html#Recording_the_computational_environment).  
- See the [Containerizing](Containerizing.html) section on how to customize and build containers.  

## Submitting jobs to the cluster

[Research IT documentation](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Documentation.aspx)  
[Submitting jobs with containers instead of modules](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Submitting-a-Batch-Job-That-Uses-Containers.aspx)  
[Slurm settings and Sumner job limits](https://jacksonlaboratory.sharepoint.com/:u:/r/sites/ResearchIT/SitePages/What%20are%20the%20Cluster%20SLURM%20Settings%20and%20Job%20Limits.aspx?csf=1&web=1&e=zWk2zy)  
[Translating PBS/Torque to Slurm](https://slurm.schedmd.com/rosetta.pdf)  
[Slurm Quick Start User Guide](https://slurm.schedmd.com/quickstart.html)  


