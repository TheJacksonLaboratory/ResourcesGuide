---
title: "Overview of computational resources"
author: "Carter Lab"
date: "2021-03-02"
---



## JAX HPC
The JAX HPC infrastructure consists of 2 main clusters available to all users: Sumner and Winter.  The Sumner cluster is our general purpose compute cluster while Winter is our GPU based cluster.
Research IT provides extensive documentation on what's available and how to use it:  
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/HPC-Quickstart.aspx  
https://github.com/TheJacksonLaboratory/IntroToHPC/blob/master/Topic%20Outline.md  
See also the [Working with HPC section](Working_with_hpc.html) in this guide and [Samir Amin's documentation](https://sumner.verhaaklab.com/).

## Storage
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/HPC-Storage-Resources.aspx
  
**tier1** is best used for standardized pipelines and research projects that are developed and deployed mainly on the cluster.  
`/home/` Personal space; by default, only the user can view and access.  
`/projects/carter-lab/` for shared projects  
`/sdata/carter-lab/` for shared projects that involve secured data  
`/fastscratch/` for temporary files. [See here for usage guidelines](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Fastscratch-Guidelines-and-Usage.aspx)

Files on tier1 can be recovered within [7 days of deletion](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Recovering-Data-from-the-.snapshot-Directory.aspx)  

**tier2** is best used for research project that are developed mainly in [Rstudio Server], as well as "cold data". Can be accessed with Globus or from RStudio Server. Projects that involve secured data should be in `/tier2/carter-lab/sdata/`

**/unixhome** is the space mounted on RStudio Server. It is limited to 50GB and therefore not suitable for big projects.

You can use [Globus](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Globus-Data-Transfers.aspx) or [SCP](https://haydenjames.io/linux-securely-copy-files-using-scp/) to transfer data between the different storages. You can also use [Globus CLI](https://docs.globus.org/cli/quickstart/) to incorporate data transfer in the script whenever applicable.  

## carterdev
Development space for out lab. Hosts shiny apps and websites for ongoing projects. To connect type http://carterdev:3838/ in your browser.

## Synapse
https://www.synapse.org/  
This is an online platform for hosting, managing, and sharing data and research projects, including results and report, in a reproducible and reusable manner. We have [a private workspace there](https://www.synapse.org/#!Synapse:syn23573590/wiki/), which can be used for any project. There is also a separate space specifically for [Model-AD](https://www.synapse.org/#!Synapse:syn7419026/wiki/586126) and [TREAT-AD](https://www.synapse.org/#!Synapse:syn21532474). You'd need to register on Synapse and then be added to the relevant team. See the [Acquiring and Sharing Data](Acquiring_and_sharing_data.html) section in this guide for more details on how to use this platform. 

## RStudio Server

1. Open a ticket with IT to request an account
2. Go to https://rstudio.jax.org/ and sign in with your jax credential  
3. To navigate to tier2 space: Click on the three dots in the upper-right corner of the files management block. In the "go to folder" pop-up enter `/tier2/carter-lab/`.  

The space provided with RStudio Server is limited in size and is threfore best to use as a dumpster/attic only. It is better to use tier2 for developing and sharing projects, especially ones that involve heavy and/or secured data.

## RStudio Connect
RStudio Connect is a publishing platform for sharing Shiny applications, R Markdown reports/websites, figures and tables, Jupyter Notebooks, etc.
You need to ask IT to add you to this server specifically. You can log into it either from RStudio Server or https://rshiny.jax.org/connect/.

## Nextflow
[Nextflow](https://www.nextflow.io/) is [a workflow management system](https://www.biorxiv.org/content/10.1101/2020.08.04.236208v1.full) that has become popular in recent years in general and [at jax](https://github.com/lifebit-ai/jax-tutorial). It is also incorporated in the [Lifebit CloudOS](https://jacksonlaboratory.sharepoint.com/sites/CloudOS) platform (used by some labs at jax but not the Carter lab at this point).

The main use of workflow management systems is in developing standardized pipelines that need to be run repeatedly (e.g., preliminary processing of RNA-seq fastq files).  
Jax has in the past developed its own workflow management framework called [civet](https://github.com/TheJacksonLaboratory/civet), with a web-based interface called [JODAP](https://bitbucket.jax.org/projects/CIV/repos/wiki/browse/home.md). However, this system has been generated based on the now-retired helix and TORQUE. Moving forward it is best to use community-based frameworks such as [CWL](https://www.commonwl.org/), [WDL](https://openwdl.org/), or [Nextflow](https://www.nextflow.io/). [The R package `targets` provides workflow management within R](https://books.ropensci.org/targets/).

[An example for how to configure a nextflow pipeline to run on sumner.](https://bitbucket.jax.org/users/peera/repos/prepare_genome/browse/nextflow.config)  
[Nextflow-based pipeline to run and deploy reproducible analyses](https://github.com/grst/universal_analysis_pipeline)  
[Demonstrations of various programming techniques for use inside Nextflow pipelines](https://github.com/stevekm/nextflow-demos)  

## JAXREG
https://jaxreg.jax.org/  
A platform for Singularity containers by JAX HPC users. The Carter lab has its own team space there. Once you login with your jax credentials, you can upload containers and create collections. You can then either make it public - open to the whole jax community - or you can make it private and invite team members and anyone else at jax.

## Jax github
The main advantage of using git and github is in its version control and the clone/fork/merge functionality that facilitates collaborative development. Main disadvantage is currently the size limitation on files and repositories. This is why github is mostly useful for developing and sharing code and text, and for research projects that are light on data files. 

The Carter lab has its own team space inside the [Jax account](https://github.com/TheJacksonLaboratory). When making new repositories you can make it under jax account and then link it to the carterlab team. This can be done by inviting the carterlab team under Settings and Manage Access within your repository. You can do the same under your own personal account, and invite anyone in or outside jax, but unless you have a premium account, you will not be able to make the repository private. Also, it will be less visible to other jax members when it's under your personal account.  
**Note:** There is no difference in the definition of *public* and *private* repositories between organization accounts and individual user accounts. If you create a public repository under the jax account, it is visible to the whole wide world.   

See [here](https://git-scm.com/book/en/v2) for git tutorial and [here](https://lab.github.com/) for github courses

## Box
Personal accounts as well as [lab's account](https://thejacksonlaboratory.ent.box.com/folder/49919058619)  
Doesn't work with RStudio projects, but good for sharing documents within the lab, across jax, and outside jax. A favorite of HR.  
If you work with RStudio Projects it's better to use Dropbox.    

## Dropbox 

* Individaul accounts only (no lab account).  
* Plays nicely with RStudio projects.  
* Allows version control and backup with unlimited size (you'd need another tool to view version differences though).  
* Files can be set up to be ignored during synchronization in either direction: they can be either [saved only on local machines](https://help.dropbox.com/files-folders/restore-delete/ignored-files) (just like .gitignore) or accesible only online throu the Smart Sync feature in Preferences (to save space on local machine).   

**Note:** only one account can be linked to one device. You can't have both personal and jax account on the same device. If you have your personal account currently linked to your jax device, you'd need to unlink that account first, and then link the jax account. Once unlilnked, all files on your personal account that have been synced to the device will still be saved on the device but no longer synced with your account.  

To get started you need to request IT to open a dropbox account for you (if you haven't been added already), activate the account via the email you'll get, download the desktop app, and get started. By default, all files are set up as Smart Sync, which means they are not saved on your local machine for offline use, so make sure you reveiw the Sync settings under Preferences. To make a file/folder available offline, right-click it in your local file viewer and choose Smart Sync -> Local

## Working remotely
In order to work remotely using jax resources, you'll need to [connect via VPN](https://jacksonlaboratory.sharepoint.com/sites/DigitalWorkspace/SitePages/How-To-Use-VPN.aspx).   
This will also provide you automatic access to full papers in any journals that jax library subscribes to.  
Alternatively, you can access the library resources [here](https://login.ezproxy.jax.org/login) without getting on the vpn. 

## Get connected, get help

* [slack](https://jacksonlaboratory.sharepoint.com/sites/IT/SitePages/How-to-use-Slack.aspx): Look for the CarterLabJax workspace and The Jackson Laboratory workspace. Once logged into a workspace, you can browse and join channels.  
* [hpctalks](https://hpctalk.jax.org/)
* [helpdesk self-service](https://jax.service-now.com/jax)
* [webex](https://jacksonlaboratory.sharepoint.com/sites/IT/SitePages/Webex-Meetings-Introduction.aspx)
* [Microsof teams](https://jacksonlaboratory.sharepoint.com/sites/DigitalWorkspace/SitePages/Teams-and-Office-365-Groups.aspx)
* [Jax computational community](https://jacksonlaboratory.sharepoint.com/sites/ComputationalCommunity?CT=1568310418608&OR=OWA-NT&CID=be6f5659-d728-0441-53b8-d1f0e03afba6)
* [Interest groups and Journal clubs](https://jacksonlaboratory.sharepoint.com/:x:/r/sites/ResearchResources/_layouts/15/Doc.aspx?sourcedoc=%7B3C15F833-3032-4560-831B-D6F02A6E6757%7D&file=Interest%20Groups%20and%20Journal%20Clubs%2009.2020.xlsx&action=default&mobileredirect=true&DefaultItemOpen=1)

## Training
[Jax Bioinformatics Training Program](https://jacksonlaboratory.sharepoint.com/sites/JAXBioinformaticsTrainingProgram)  
[Jax Research IT documentation](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Documentation.aspx)  
[Jax videos](http://jaxbhflash02.jax.org/index/default.aspx)   
[Software Carpentry lessons](https://carpentries.org/community-lessons/)  
[Software Carpentry on Programming with R](https://swcarpentry.github.io/r-novice-inflammation/)  
[HPC Carpentry lessons](https://www.hpc-carpentry.org/)  
[RStudio lessons](https://education.rstudio.com/)  
[GitHub lessons](https://lab.github.com/)  
[The Data Management Skillbuilding Hub by DataOneOrg](https://dataoneorg.github.io/Education/)  
[Happy Belly Bioinformatics](https://astrobiomike.github.io/)  
[Data Science Textbook by Rafael A. Irizarry](https://rafalab.github.io/dsbook/)  
[Reproducible Research by Karl Broman](https://kbroman.org/steps2rr/)  
[Computational Genomics with R by Altuna Akalin](https://compgenomr.github.io/book/)  
[What they forgot to teach you about R by Jenny Bryan](https://rstats.wtf/)
[Happy git with R by Jenny Bryan](https://happygitwithr.com/)

![image](images/googling.jpg)