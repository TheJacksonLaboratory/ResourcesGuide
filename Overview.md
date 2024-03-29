---
title: "Overview of Computational Resources"
author: "Annat Haber"
date: "2023-06-22"
---



## [JAX HPC](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/HPC-Quickstart.aspx)
The JAX HPC infrastructure consists of 2 main clusters available to all users: Sumner and Winter.  The Sumner cluster is our general purpose compute cluster while Winter is our GPU based cluster.
Research IT provides extensive documentation on what's available and how to use it.  
[HPC Quickstart](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/HPC-Quickstart.aspx)  
[Intro to HPC](https://github.com/TheJacksonLaboratory/IntroToHPC/blob/master/Topic%20Outline.md)  
See also the [Working with HPC section](Working_with_hpc.html) in this guide and [Samir Amin's documentation](https://code.sbamin.com/hpc).

## [Storage](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/HPC-Storage-Resources.aspx)
  
**tier1** is best used for standardized pipelines and research projects that are developed and deployed mainly on the cluster.  
`/home/` Personal space; by default, only the user can view and access.  
`/projects/carter-lab/` for shared projects  
`/sdata/carter-lab/` for shared projects that involve secured data  
`/fastscratch/` for temporary files. [See here for usage guidelines](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Fastscratch-Guidelines-and-Usage.aspx)

Files on tier1 can be recovered within [7 days of deletion](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Recovering-Data-from-the-.snapshot-Directory.aspx)  

**tier2** is best used for research project that are developed mainly in [Rstudio Server], as well as "cold data". Can be accessed with Globus or from RStudio Server. Projects that involve secured data should be in `/tier2/carter-lab/sdata/`

**/unixhome** is the space mounted on RStudio Server. It is limited to 50GB and therefore not suitable for big projects.

You can use [Globus](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Globus-Data-Transfers.aspx) or [SCP](https://haydenjames.io/linux-securely-copy-files-using-scp/) to transfer data between the different storages. You can also use [Globus CLI](https://docs.globus.org/cli/quickstart/) to incorporate data transfer in the script wherever applicable.  

## [carterdev](http://carterdev:3838/)
Development space for out lab. Hosts shiny apps and websites for ongoing projects.

## [Synapse](https://www.synapse.org/)  
This is an online platform for hosting, managing, and sharing data and projects, including results and reports, in a reproducible and reusable manner. [The AD Knowledge Portal](https://adknowledgeportal.synapse.org/) uses synapse for its infrastructure. Our lab has [a private workspace there](https://www.synapse.org/#!Synapse:syn23573590/wiki/), which can be used for any project. There are also separate spaces specifically for [Model-AD](https://www.synapse.org/#!Synapse:syn7419026/wiki/586126) and [TREAT-AD](https://www.synapse.org/#!Synapse:syn21532474). You'd need to register on Synapse and then be added to the relevant team. See [here](Acquiring_and_sharing_data.html) for more details. 

## [Climb.bio](https://climb.bio/)
A Lab Information Management System specifically designed for colony management of lab animals, used by MODEL-AD and Marmo-AD to manage their colonies. If you work with datasets generated by these consortia, it's a good idea to get animal metadata directly from climb rather than relying on files from other people. Information from climb can be downloaded manually from the web app, or programmatically using the [climb API](https://api.climb.bio/docs) and its [R client](https://github.com/TheJacksonLaboratory/ClimbR). 

## [RStudio Server](https://rstudio.jax.org/)
1. Open a ticket with IT to request an account
2. Go to https://rstudio.jax.org/ and sign in with your jax credential  
3. To navigate to tier2 space: Click on the three dots in the upper-right corner of the files management block. In the "go to folder" pop-up enter `/tier2/carter-lab/`. The space provided with RStudio Server is limited in size. It is therefore best to use tier2.

## [RStudio Connect](https://rshiny.jax.org/connect/)
RStudio Connect is a publishing platform for sharing Shiny applications, [R Markdown reports/websites](Publishing.html#Making_websites_from_Rmarkdown_files), figures and tables, Jupyter Notebooks, etc.
You need to ask IT to add you to this server specifically. You can log into it either from RStudio Server or https://rshiny.jax.org/connect/.

## [Nextflow](https://www.nextflow.io/)
[Nextflow](https://www.nextflow.io/) is [a workflow management system](https://www.biorxiv.org/content/10.1101/2020.08.04.236208v1.full) that has become popular in recent years in general and [at jax](https://github.com/lifebit-ai/jax-tutorial). It is also incorporated in the [Lifebit CloudOS](https://jacksonlaboratory.sharepoint.com/sites/CloudOS) platform (used by some labs at jax but not the Carter Lab at this point).  
NF-Core is a community-based collection of standardized pipelines built with nextflow. MODEL-AD and Marmo-AD use nf-core pipelines for processing [bulk RNAseq](https://www.synapse.org/#!Synapse:syn7419026/wiki/611613) and [WGS](https://www.synapse.org/#!Synapse:syn23573590/wiki/614361) data.

[A universal config file to run on Jax HPC](https://github.com/TheJacksonLaboratory/universal-nextflow-config)
[Another example for how to configure a nextflow pipeline to run on sumner.](https://bitbucket.jax.org/users/peera/repos/prepare_genome/browse/nextflow.config)  
[Nextflow-based pipeline to run and deploy reproducible analyses from jupyter/RMarkdown notebooks](https://github.com/grst/universal_analysis_pipeline)  
[Demonstrations of various programming techniques for use inside Nextflow pipelines](https://github.com/stevekm/nextflow-demos) 
[Installing edge releases](https://nf-co.re/viralrecon/dev/usage#nextflow-edge-releases)

## [JAXREG](https://jaxreg.jax.org/)
A platform for Singularity containers by JAX HPC users. The Carter lab has its own team space there. Once you login with your jax credentials, you can upload containers and create collections. You can then either make it public - open to the whole jax community - or you can make it private and invite team members and anyone else at jax.

## [Jax GitHub](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/JAX-Github.aspx)
The main advantage of [using git and github](Organizing_projects.html#Working_with_Git_and_GitHub) is in its version control and the clone/fork/merge functionality that facilitates collaborative development. Main disadvantage is currently the size limitation on files and repositories. This is why github is mostly useful for developing and sharing code and text, and for research projects that are light on data files. 

The Carter Lab has its own team space inside the [Jax account](https://github.com/TheJacksonLaboratory). When making new repositories you can make it under jax account and then link it to the carterlab team. This can be done by inviting the carterlab team under Settings and Manage Access within your repository. You can do the same under your own personal account, and invite anyone in or outside jax, but unless you have a premium account, you will not be able to make the repository private. Also, it will be less visible to other jax members when it's under your personal account.  
**Note:** There is no difference in the definition of *public* and *private* repositories between organization accounts and individual user accounts. If you create a public repository under the jax account, it is visible to the whole wide world.   

See [git tutorial](https://git-scm.com/book/en/v2) and [github courses](https://lab.github.com/).  

## [Box](https://thejacksonlaboratory.ent.box.com/folder/49919058619)
Personal accounts as well as [lab's account](https://thejacksonlaboratory.ent.box.com/folder/49919058619) are available through jax.  
Box doesn't work with RStudio projects, but good for sharing documents within the lab, across jax, and outside jax. A favorite of HR.  
If you work with RStudio Projects it's better to use Dropbox.    

## [Dropbox](https://help.dropbox.com) 

* Individaul accounts only (no lab account).  
* Plays nicely with RStudio projects.  
* Allows version control and backup with unlimited size (you'd need another tool to view version differences though).  
* Files can be set up to be ignored during synchronization in either direction: they can be either [saved only on local machines](https://help.dropbox.com/files-folders/restore-delete/ignored-files) (just like .gitignore) or accesible only online throu the Smart Sync feature in Preferences (to save space on local machine).   

**Note:** only one account can be linked to one device. You can't have both personal and jax account on the same device. If you have your personal account currently linked to your jax device, you'd need to unlink that account first, and then link the jax account. Once unlilnked, all files on your personal account that have been synced to the device will still be saved on the device but no longer synced with your account.  

To get started you need to request IT to open a dropbox account for you (if you haven't been added already), activate the account via the email you'll get, download the desktop app, and get started. By default, all files are set up as Smart Sync, which means they are not saved on your local machine for offline use, so make sure you reveiw the Sync settings under Preferences. To make a file/folder available offline, right-click it in your local file viewer and choose Smart Sync -> Local

## [Working Remotely](https://jacksonlaboratory.sharepoint.com/sites/DigitalWorkspace/SitePages/What-is-the-Jax-Digital-Workspace.aspx)
In order to work remotely using jax resources, you'll need to [connect via VPN](https://jacksonlaboratory.sharepoint.com/sites/DigitalWorkspace/SitePages/How-To-Use-VPN.aspx).   

## [Jax Library](https://jacksonlaboratory.sharepoint.com/sites/Library)
In order to access [Jax library](https://jacksonlaboratory.sharepoint.com/sites/Library) off-campus you'll need to [connect via VPN](https://jacksonlaboratory.sharepoint.com/sites/DigitalWorkspace/SitePages/How-To-Use-VPN.aspx). This will also provide you automatic access to full papers in any journals that jax library subscribes to.  
Alternatively, you can access the library resources [here](https://login.ezproxy.jax.org/login) with your jax credentials, without getting on the vpn

## [Get Connected, Get Help](https://jax.service-now.com/jax)

* [slack](https://jacksonlaboratory.sharepoint.com/sites/IT/SitePages/How-to-use-Slack.aspx): Look for the CarterLabJax workspace and The Jackson Laboratory workspace. Once logged into a workspace, you can browse and join channels.  
* [hpctalks](https://hpctalk.jax.org/)
* [helpdesk self-service](https://jax.service-now.com/jax)
* [Microsof teams](https://jacksonlaboratory.sharepoint.com/sites/DigitalWorkspace/SitePages/Teams-and-Office-365-Groups.aspx)
* [Jax computational community](https://jacksonlaboratory.sharepoint.com/sites/ComputationalCommunity?CT=1568310418608&OR=OWA-NT&CID=be6f5659-d728-0441-53b8-d1f0e03afba6)
* [Interest groups and Journal clubs](https://jacksonlaboratory.sharepoint.com/:x:/r/sites/ResearchResources/_layouts/15/Doc.aspx?sourcedoc=%7B3C15F833-3032-4560-831B-D6F02A6E6757%7D&file=Interest%20Groups%20and%20Journal%20Clubs%2009.2020.xlsx&action=default&mobileredirect=true&DefaultItemOpen=1)

## [Training](https://jacksonlaboratory.sharepoint.com/sites/JAXBioinformaticsTrainingProgram)
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
[Data visualization by Karl Broman](https://www.youtube.com/watch?v=Ssso_5X1UPs&t=63s)  

![image](images/googling.jpg)
