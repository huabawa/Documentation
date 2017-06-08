---
title: Course Notebooks
keywords: cloud
last_updated: October 6, 2016
tags: [research_computing]
summary: "Notebooks for students in coursework"
sidebar: mydoc_sidebar
permalink: curr_notebooks.html
folder: curr
---

## Introduction


This curriculum page describes notebook use by students for coursework with emphasis on Jupyter 
notebooks and Python. There are several desirable features to cover including...


- students authenticate to their copy of the Notebook via their usual campus login
- students have a read/clone view of the instructor(s) Master notebook
- student work is easily shared with and reviewed by the instructor(s)
- data files persist in all cases, even if the notebook is shut down
- the notebook is running an engine that executes code (not just a static view of already-executed material)
- instructor has simple administrative interface to the Master and granting students access
- Master is under source control; can be modified directly or via pull request
- Notebook supports markdown, LaTeX, animation, checkpoint/rollback, other 'nice' features
- Notebook has a **How To** pre-installed with...
  - curl, !shell execution and other common tasks by example
  - file access: open / read / write also by example
  - administrative tasks for the host system and notebook e.g. % sudo yum update
  - data access to non-local resources: Cloud storage, API calls etc by example


## Links


- [Linux cURL command](http://www.computerhope.com/unix/curl.htm)
- [Access to Jupyter notebooks via ssh](https://coderwall.com/p/ohk6cg/remote-access-to-ipython-notebooks-via-ssh)


## Yo


- ***As always: Key files that arrive in GitHub repos are still available after deletion via versioning. Don't do it!***
- ***Our [glossary](cc_glossary.html) may help with unfamiliar terms.***
- ***Search cloudmaven for 'tunneling' for more on connecting to notebooks through security barriers.***
- Jupyter sub-themes
  - Jupyter notebook
  - Jupyter Lab
  - Jupyter Hub
  - 


## Dialog


The following is a discussion between Professors A, B, M, and R together with program manager Kilroy.


Prof A: Are we doing anything to manage Jupyter notebooks for coursework at the university? I would like to be able to set
homework and projects up for the students together with associated data. Students log in using their University ID and go
from there, writing and executing test code in a matter of minutes.  I can see their work so that work must persist after
the students log off; and this implies the organizing structure is around the course, not the students. JupyterLab has some
of this already built in as a starting point. 


Kilroy: Awesome, let's do it. Adding additional faculty with similar interests. Before they chime in let me mention that
Microsoft Azure is hosting Jupyter notebooks as a free service at this time; where the features are evolving as they go but the
notebook content does persist (Master or Clone) under ownership via Windows LiveID. Data files may need re-loading via
curl; and the management of a large class could get tricky. I really appreciate Microsoft taking this approach and clearly
their goal is to make the Azure stack more accessible; so that's great. 


Kilroy: As an alternative to Azure we could also create a dedicated cloud-based Notebook service. It would be pretty 
inexpensive for a single class provided that not a lot of compute/memory horsepower was needed. We could enlist the 
folks at University IT to help build this in a robust way, up from a POC. The NetID authentication is already done (by
those same folks). 


Prof B: Would GitHub work for this? Perhaps there are FERPA compliance issues; but the good thing is that version control
is already in place. ([Here is a link](https://github.com/blog/1995-github-jupyter-notebooks-3).)


Prof M: We use GitLab, a locally-installed version of GitHub. This gives us unlimited private repositories, i.e. one 
repo per student x course. They pull content from an upstream repo that the instructors populate; and they submit
assignments by push. 


Prof R: A valuable aspect of the Azure notebook server or [Binder](http://mybinder.org) is that students run the notebooks
without having to install the Jupyter software stack (Python etcetera) on their own machine, not to mention dependencies
and things break. GitHub / GitLab (my understanding) is static-only so that goes against the interactivity of student 
contribution. We're running into these issues in the process of writing a book; so we'd really support running a server.


Prof B: Agreed. Pre-rendered (static) notebooks match well to GitHub; but having the full-blown server working 
provides the feature set we want at the cost of more effort.  


Prof R: Another idea is to use a machine image such as a Docker file. Students spin this up quickly on a VM, do their 
work and save the revised image; which could be viewed by the instructor for evaluation.  Also a bit tangential here are
some related references:


- [Pacific Inst of Math Sciences: Jupyter server](https://www.computecanada.ca/featured/compute-canada-and-pims-launch-jupyter-service-for-researchers/)
  - This may prove to be available only to PIMS researchers, not for general audiences
- [NERSC supercomputer notebook interface](http://www.nersc.gov/news-publications/nersc-news/nersc-center-news/2016/jupyter-notebooks-will-open-up-new-possibilities-on-nerscs-cori-supercomputer)
- [TACC supercomputer notebook hosting](https://www.tacc.utexas.edu/-/why-use-jupyter-notebooks-in-designsafe)
= [SDSC supercomputer notebook repo](https://zonca.github.io/2015/09/ipython-jupyter-notebook-sdsc-comet.html)


{% include links.html %}