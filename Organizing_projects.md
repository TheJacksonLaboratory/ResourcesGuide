---
title: "Organizing Projects"
author: "Carter Lab"
date: "2021-03-22"
---


## FAIR Principles

In 2016 a consortium of scientists and organizations published ["The FAIR Guiding Principles for scientific data management and stewardship”](https://www.nature.com/articles/sdata201618
). It has since been endorsed by [G20](https://www.dtls.nl/2016/09/13/g20-endorse-fair-principles/), [library associations](https://libereurope.eu/blog/2017/12/08/implementing-fair-data-principles-role-libraries/), and [many other](https://en.wikipedia.org/wiki/FAIR_data#Acceptance_and_implementation_of_FAIR_data_principles) [research organizations](https://www.sciencedirect.com/science/article/pii/S1359644618303039).  
[FAIR](https://www.go-fair.org/fair-principles/) stands for Findable, Accessible, Interoperable, Reusable

**Motivation:**  
From the FAIR publication:  
“Beyond proper collection, annotation, and archival, data stewardship includes the notion of ‘long-term care’ of valuable digital assets, with the goal that they should be reusable for downstream investigations, either alone, or in combination with newly generated data... The principles apply not only to ‘data’ in the conventional sense, but also to the algorithms, tools, and workflows that led to that data: all components of the research process must be available to ensure transparency, reproducibility, and reusability.”

To that end, we can think of a computational project as a data object in the broad sense  - an identifiable digital item that includes data elements such as tables of measurements along with all the relevant metadata and scripts - and apply FAIR principles to all aspects of the project, including organization, development, and publication. Project components may be of any level of complexity and appear in any form or syntax. Documentation of wet-lab components can and should be included in some form as well, as they are part of the data provenance.  

**Goals:**

* To allow both humans and machines to make sense of the data as is
* To allow the reader/collaborator to reproduce the whole paper/analysis with a click of a button (or a few)
* To provide a record of our *in silico* experiments similar to [a wet-lab notebook](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1004385)

Here are some rules of thumb for how to set up and organize a FAIR-compliant project. See below for more details. See other sections for acquiring and sharing data and for publishing.

**Rules of thumb for [a project-oriented workflow](https://rstats.wtf/project-oriented-workflow.html):**

* Set up your project folder as a formal R project in RStudio, or the equivalent in other IDEs  
  * In Rstudio: set 'save workspace on exit' to 'never' (Preferences -> General)
* Use standardized naming and structure following [community conventions](https://community.rstudio.com/t/best-practice-for-good-documented-reproducible-analysis/1995/)
* Keep raw data separate from scripts and output (more details in [Acquiring and Sharing Data](Acquiring_and_sharing_data.html)
* Treat input data as read-only and output as disposable. [Script everything](https://kbroman.org/steps2rr/pages/scripts.html), and [save scripts, not workspaces](https://rstats.wtf/save-source.html)  
* Include a “dumbed-down” [README file](https://data.research.cornell.edu/content/readme) as a walk-through (maybe a flow chart; maybe md file)  
* Include a license file as needed; CC-0 and CC-BY are [commonly used](https://blog.datadryad.org/2017/09/11/some-dos-and-donts-for-cc0/)  
* If your project involves working on several file systems, it's a good idea to duplicate whole project folders, not bits and pieces of data/code.  
  * You can sync your local copy with Box/Dropbox for extra backup and version control (note: box doesn't work with RStudio projects).
  * Options for synchronizing your local and remote copies: Globus, FileZila, [rsync](https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/Transferring-Files-to-and-from-the-Cluster-with-rsync.aspx)  
  * You can use [git](https://git-scm.com/book/en/v2) for version control and sharing of code.  

**Other good references:**  
https://riojournal.com/article/56508    
https://cran.r-project.org/web/views/ReproducibleResearch.html  
https://kbroman.org/steps2rr/   
https://community.rstudio.com/t/best-practice-for-good-documented-reproducible-analysis/1995/2   
https://www.tandfonline.com/doi/abs/10.1080/00031305.2017.1375986?journalCode=utas20  
https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424  
https://old.dataone.org/sites/all/documents/DataONE_BP_Primer_020212.pdf  
  
## R projects and Rmarkdown

If you're working with [RStudio](https://rstudio.com/products/rstudio/), you can set up your project folder as [a formal R project](https://support.rstudio.com/hc/en-us/articles/200526207-Using-Projects). This makes it easier to combine analyses into one complex project in a modular and self-contained manner. RStudio also provides built-in functionality for version control of projects with git, running multiple projects in parallel, and other useful features. Other IDEs have their own set up, e.g. [Visual Studio](https://code.visualstudio.com/docs/setup/setup-overview).

[RMarkdown](https://bookdown.org/yihui/rmarkdown/) is a file type [adopted by RStudio](https://rmarkdown.rstudio.com) for making [reproducible reports](https://kbroman.org/steps2rr/pages/reports.html) using [the markdown language](https://guides.github.com/features/mastering-markdown/). This means that you can document your whole analysis, combining text, code, and results all in one file, and re-run it all with a click of a button. The same can be achieved with e.g., [jupyter notebooks](https://jupyter.org/try). Both RMarkdown and jupyter notebooks can run code in many languages, not just R or python, which makes it easy to document all steps of your analysis in one file in the right sequence.  

When Rmarkdown files are rendered they can be converted into several formats. The most commonly used is html. See [here](https://bookdown.org/yihui/blogdown/output-format.html) for format comparison. If you push your project to GitHub, it might be useful to have a markdown version of your report, which can then be viewed on [GitHub](#Working with Git and GitHub) as a rendered html-like report. That basically gives you an easy way to publish your reports.
To save a markdown version while rendering your report as html, add `keep_md:true` to the yaml frontmatter (the text at the top of your file between leading and trailing lines of ---) like this:
```
output: 
  html_document:
    keep_md: true
```
To save a markdown version without rendering as html you'd simply replace the output section with `output: github_document`.  

You don't have to use RStudio to generate reports with R Markdown, you can use the `rmarkdown` package in any R console, but RStudio provides integrated functionality that makes it easier and promotes standardization and reproducibility.

Two other major advances in the R world in recent years include the [tidyverse](https://www.tidyverse.org/) and [ggplot](https://ggplot2.tidyverse.org/index.html) approaches to data analysis and visualization, respectively. Although they involve quite a learning curve, they make R code more streamlined and readable, and overcome some default behaviors in R that have become a nuisance in the new age of data science (see [here](https://peerj.com/preprints/3180v1/) for examples).  

For a more effective use of R and RMarkdown see [What They Forgot to Teach You About R by Jenny Bryan](https://rstats.wtf/)

## Folder structure and naming
Using community standards for structuring projects and naming files and folders makes it easier for other people to understand your workflow, with or without a readme file. There are several good discussions out there, and several R packages that help setting up a standardized R project. The principles are the same for other languages and IDEs.

Some examples: [ProjectTemplate](http://projecttemplate.net/), [prodigenr](https://cran.r-project.org/web/packages/prodigenr/vignettes/prodigenr.html), [cookiecutter](http://drivendata.github.io/cookiecutter-data-science/)  
http://kbroman.org/steps2rr/pages/organize.html  
https://chrisvoncsefalvay.com/2018/08/09/structuring-r-projects/  
https://cran.r-project.org/web/views/ReproducibleResearch.html  

Below are two examples for conventional project structure and naming  

![image](images/projectStructure.png)

## Recording the computational environment
The R package [`renv`](https://rstudio.github.io/renv/articles/renv.html) provides a good way to record the R version and all the packages that your project requires to run, [regardless of whether you end up dockerizing your project or not](https://rstudio.github.io/renv/articles/docker.html). This is the newest incarnation of `packrat`, by the same developers, so if you've been using `packrat`, it's a good idea to upgrade to renv.

A few pointers:

* call `renv::init()` to initialize a new project-specific environment with a private R library for a given project:
  * saves all currently loaded packages into a lockfile
  * updates the `.Rprofile` file so that it activates `renv`, which then looks up the lockfile, every time you restart a session within that project (so no need to tell other/future users to activate renv)
* call `renv::snapshot()` to update the package list (lockfile) while working on the project
* call `renv::restore()` to install all packages in the lockfile (e.g., when starting a session on a different system); inform users of the need to do that
* Instead of installing a new copy of each package for each project, renv creates a "playlist" for each project that draws from the user library. If it can't find it there, it will install it in the project library by default. See the function's reference to change that default.
* To install Bioconductor packages, use `renv::install("bioc::Biobase")`. To include Bioconductor packages when initializing or restoring a project enter `options(repos = BiocManager::repositories())` first. 
* Integrates python as well: https://rstudio.github.io/renv/articles/python.html  

## Working with Git and GitHub
[Intro to version control and git](https://www.atlassian.com/git/tutorials/what-is-version-control)  
[The entire Pro Git book](https://git-scm.com/book/en/v2)  
[Into to GitHub](https://lab.github.com/githubtraining/introduction-to-github)  
[Happy git with R by Jenny Bryan](https://happygitwithr.com/)  
[Troubleshooting Git](https://ohshitgit.com/)  
[Publishing RMarkdown websites on Github Pages](https://resources.github.com/whitepapers/github-and-rstudio/)  

One way to get started -   
**Pushing an existing project folder to a new remote repo:**  

1. Create a new remote (empty) repository under [the Jackson Laboratory account](https://github.com/TheJacksonLaboratory)  
  -- do not initialize with README/.gitignore/license
2. Initialize your project folder as a local git repo and link it to the remote one:
```
cd myproject
git init
git remote add origin https://github.com/TheJacksonLaboratory/myproject.git
```
3. Track and commit everything that's currently in the project folder, and push to the remote repo
```
git add .
git commit -m "initial commit"
git push -u origin main
```
4. Make any changes and repeat step 3 (with a different and informative commit message).
5. Pull (fetch and merge) any changes that someone else may have pushed to the remote repo since the last time you worked on it with `git pull`

**Note:** If you set up your project folder as an RStudio project you have the option to initiate it as a git repo from the outset. In this case you just need to do the `git remote add` command in step 1. You will then have a .gitignore file already in there. It's a good idea to add LICENSE and README files as well.  
See [here](https://labs.consol.de/development/git/2017/02/22/gitignore.html) for a good reference on how to write `.gitignore`.  
  
Another way to get started -  
**Cloning an existing remote repo to a new local folder:**

1. Clone the repo from github to your local machine
```
git clone https://github.com/TheJacksonLaboratory/someproject.git
```
2. Make changes and repeat step 3 above
3. Repeat step 5 above

If you're going to collaborate on code/text development through github, it helps to [understand the github flow](https://guides.github.com/introduction/flow/) and [how branching works](https://www.freecodecamp.org/news/git-clone-branch-how-to-clone-a-specific-branch/). It is also a good idea to work with a graphic client such as the one provided in [RStudio](https://jennybc.github.io/2014-05-12-ubc/ubc-r/session03_git.html#learngit) and Visual Studio, or the stand-alone [GitHub Desktop](https://desktop.github.com/), rather than working from the command line. This is especially useful for monitoring and managing complex merges. 
