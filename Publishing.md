---
title: "Publishing"
author: "Carter Lab"
date: "2020-12-14"
---



## Findable and Accessible: Where to publish?

* As a supplement: simple but not not standardized and easy to overlook, lose, and mishandle  
https://www.the-scientist.com/news-opinion/the-push-to-replace-journal-supplements-with-repositories--66296

* As a separate product:  
Increases accessibility and reproducibility, promotes detailed descriptions as well as credit  
  A DOI-issuing repository is better than URL for permanency  
  A registry of repositories: https://www.re3data.org/  
  Examples:  
    * Mendeley: centralized data repository, minimal description, not standardized  
    * Dryad: centralized data repository with peer review  
    * ResearchGate: social media for scientists  
    * Data-in-Brief: thorough and standardized description of data and methods  
    * MethodX: the nitty-gritty of the methods (computational and experimental)  
    * Github: for code mostly  
    * Zenodo: for development and review as well as publication, integrates with github  

## Interoperable and reusable: What to publish?

* A shiny app  
  * Good as an additional tool, not in lieu of publishing the code for the whole paper/analysis  
  * Does not provide a log of steps and parameters leading to a given result  
  * No transparency (code is locked behind GUI)  
  * No permanency (depends on availability of the specific app)  
  * Package ```shinymeta``` alleviates the above, but this is still not a full record of a given study  

* An R package 	
  * https://www.tandfonline.com/doi/abs/10.1080/00031305.2017.1375986?journalCode=utas20   
  * Not so great for development/collaboration, but good for final publication  
  * Standardized structure and built-in quality control (sometimes too much)  

* A zip file of the whole project folder, including rendered markdown reports  
  * Simple  
  * A full record of the study from A to Z  
  * Standardization is up to you

* A website	https://rmarkdown.rstudio.com/lesson-13.html  
  Centralized, with the option to link to the original files on github/box/etc – keeps everything together. See below

* A website directly from your github repo for code and reports (https://lab.github.com/githubtraining/github-pages)

### Making websites from Rmarkdown files

Rmarkdown files that are rendered as html files can be wrapped together into a website, including links to the original project folder, any associated rshiny apps, github repo, etc. This is a convenient option for making your work accessible to collaborators, especially non-computational ones. To build a website you need two additional files in the same folder as the rest of your .Rmd files: 1.) index.Rmd and 2.) _site.yml. The index.Rmd file is essentially the home page of the website. The file has to be named index.Rmd (or index.md) in order for the website to work. The _site.yml file defines the structure of the website. This is where you can specify which files, what menus/submenus, and what other information will be on the website. This website contains static htmls, so everytime the site yaml file is updated all of the rmarkdowns will be rendered again (see Rmarkdown cache options to somehwat alleviate this feature).

See [here](https://bookdown.org/yihui/rmarkdown/rmarkdown-site.html) for the basics of rendering sites from Rmarkdown files. 

More evolved (and involved) ways to build an R-based websites include the packages [workflowr](https://jdblischak.github.io/workflowr/index.html) and [distill](https://rstudio.github.io/distill/)

Below is an example of a yml file with multiple menus and submenus, as well as links for a shiny app, dropbox, github, a link to email, and various outputs.

```
name: "my-website"
navbar:
  title: "2DG C57BL/6J"
  type: "inverse"
  left:
    - text: "Home"
      href: index.html
    - text: RNAseq
      menu:
      - text: "Sample Description"
        href: RNA_seq_sample_description.html
      - text: "Data Diagnostics"
        href: 00_Chang_B6_96hr_4WK_2DG_BASIC_DIAGNOSTICS_parent.html
      - text: "Distribution Plots"
        href: 00_Chang_B6_4WK_2DG_DISTRIBUTION_PLOTS.html
      - text: "WGCNA"
        menu:
          - text: "Heart"
            menu:
            - text: "Cleaning"
              href: 00_Chang_B6_4WK_2DG_WGCNA_cleaning_heart.html
            - text: "Module Creation"
              href: 01_Chang_B6_4WK_2DG_WGCNA_MODULES_heart.html
            - text: "Pathway Analysis"
              href: 02_Chang_B6_4WK_2DG_WGCNA_PATHWAY_ANALYSIS_heart.html 
            - text: "Contribution of Main Effects to Modules"
              href: 03_Chang_B6_4WK_2DG_WGCNA_contribution_heart.html
            - text: "Contribution of Samples to Modules"
              href: 04_Chang_B6_4WK_2DG_WGCNA_Module_Sample_Contribution_heart.html
            - text: "Summary"
              href: 05_Chang_B6_4WK_2DG_WGCNA_summary_heart.html
            - text: "Cytoscape"
              href: 07_Chang_B6_4WK_2DG_WGCNA_Cytoscape_export_heart.html
            - text: "Pathway frequency across trait related modules"
              href: 08_Chang_B6_4WK_2DG_WGCNA_frequency_of_pathways_across_modules_related_to_traits_heart.html
            - text: "Hub Genes"
              href: 09_Chang_B6_4WK_2DG_WGCNA_hub_genes_heart.html
            - text: "Gene significance"
              href: 10_Chang_B6_4WK_2DG_WGCNA_gene_significance_heart.html
            - text: "Eigenmetabolite Stratification"
              href: 11_Chang_B6_4WK_2DG_WGCNA_eigenmetabolite_stratification_heart.html
    - text: Proteomics
      menu:
       - text: "Sample Description"
       - text: "Data Diagnostics"
       - text: "Distribution Plots"
       - text: "WGCNA"

  right:
    - icon: fa-connectdevelop
      href: http://carterdev:3838/2DG_SLE/
    - icon: fa-bitbucket
      href: https://bitbucket.jax.org/projects/CB/repos/sle_b6_2dg_aew/browse
    - icon: fa-dropbox
      href: https://www.dropbox.com/home/Chang_20190406_RNAseq_B6_96hr_4wk_2DG
    - icon: fa-paper-plane
      href: mailto:<ann.wells@jax.org, lucas.chang@jax.org>
      
output:
  html_document:
    theme: cerulean
    highlight: textmate
    include:
      after_body: footer.html
#      in_header: jax_logo.html
    exclude:
      - "*.RData"
      - "Data"
      - "SLE_shiny"
      - "Results"
      - "*.cys"
```
