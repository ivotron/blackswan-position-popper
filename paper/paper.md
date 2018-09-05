---
title: "Spotting Black Swans With Ease: The Case for a Practial Reproducibility Platform"
shorttitle: Black Swan
author: Ivo Jimenez and Carlos Maltzahn (UC Santa Cruz)
abstract: |
  Advances in agile software delivery methodologies and tools 
  (commonly referred to DevOps) have not yet been materialized in 
  academic scenarios such as universities and research laboratories. 
  In this position paper we make the case for _Black Swan_, a platform 
  for the agile implemention, maintainenance and curation of 
  experimentation pipelines by embracing a DevOps approach.
---

# Introduction

Reproducibility is the cornerstone of the scientific method. Yet, in 
computational and data science domains, a gap exists between current 
practices and the ideal of having every new scientific discovery be 
_easily_ reproducible [@donoho_reproducible_2009]. Advances in 
computer science (CS) and software engineering slowly and painfully 
make their way into these domains, even in (paradoxically) CS research 
[@fursin_collective_2013 ; @collberg_repeatability_2016].

Popper [@jimenez_popper_2017] is an experimentation protocol and CLI 
tool for implementing experimentation pipelines following a DevOps 
approach. The goal of Popper is to bring the same methods and tools 
used for the agile delivery of software, known as DevOps 
[@kim_devops_2016], to scientists and industry researchers.

Our experience in the past four years developing and evangelizing the 
use of Popper in multiple scientific domains has allowed us to 
identify opportunities where open-source software (OSS) can be used to 
close the existing gap in current research practices. While the Popper 
protocol is relatively easy to follow (wrap experimentation pipelines 
in the form of a sequence of Bash scripts), for many practitioners it 
still represents a big leap between current practices and where OSS 
development communities are today (automated, portable and versioned 
software testing pipelines). To this end, in this position paper we 
make the case for building _Black Swan_[^name], a platform for 
_practical_ reproducible research. In a nutshell, Black Swan enables 
the agile delivery of software created in universities and other 
research institutions, significantly accelerating technology transfers 
between research and operation.

This paper is organized as follows. Next section ([@Sec:motivation]) 
expands on the need for _Black Swan_, while section [@Sec:components] 
presents the components of the proposed platform and how they address 
the current gaps. Section [@sec:cases] illustrates the utility of the 
platform by describing use cases where Black Swan would be used. We 
close with related work and how Black Swan is different to other 
existing software ([@sec:related]).

[^name]: Black swans are typically used to illustrate the concept of 
falsifiability: the statement "all swans are white" is proven false 
whenever one can spot one or more black swans. We envision _Black 
Swan_ (the platform) to be a tool for researchers to _easily_ find 
black swans in their computational or data science theories (i.e. 
identify when their claims are false) and, more importantly, allow 
them to investigate **why**.

# Motivation {#sec:motivation}

Following the Popper convention begins by defining a pipeline for a 
scientific exploration, i.e. a sequence of high-level steps that are 
carried out when executing an experiment or analysis. For example, a 
data analysis pipeline may consist of four stages: (1) obtain a 
dataset; (2) pre-process the data; and (3) run an analysis on the 
data; (4) produce plots.

The contents of a folder containing scripts for a pipeline are shown 
in @Lst:pipe (many more examples are available on 
Github[^popperized]). Each stage in a pipeline corresponds to a Bash 
script, and it codifies what a person would manually type in a 
terminal otherwise. A stage in a pipeline is a relatively simple list 
of steps, where each invokes other tools, and passes scripts to them, 
which are themselves stored in the same repository.

```{#lst:pipe caption="Contents of a pipeline."}
paper-repo/pipelines/gassyfs
|- README.md
|- baseliner
|  \  config.yml
|- docker
|  |- Dockerfile
|  \  entrypoint.sh
|- geni
|  └── cloudlab_request.py
|- results
|  |- output.csv
|  \  visualize.ipynb
|- run.sh
|- setup.sh
|- teardown.sh
\  validate.sh
```

Thus, the Popper convention can be summarized in three high-level 
guidelines:

 1. At each stage of Pick one or more tools from the DevOps toolkit 
    [^devopstoolkit] for each stage of the scientific experimentation 
    workflow.
 2. Put all associated scripts (experiment and manuscript) in version 
    control, in order to provide a self-contained repository.
 3. Document changes as an experiment evolves, in the form of commits 
    to the corresponding repository storing the contents of the 
    pipeline (the scripts).

There are four key aspects that make Popper pipelines practical. (1) They 
are version-controlled, which allows readers to understand what was 
done over time (and why), mimicking a lab notebook; (2) 
thanks to virtualization technology at the language-, OS- or 
hardware-level, they are portable and can be easily re-executed in 
many environments; (3) they are automated; and (4) they are 
self-contained.

Since a Popper pipeline is, fundamentally, a list of Bash scripts 
(with a specific execution order) stored in a version control 
repository with a pre-defined folder structure, one would think that 
it is straightforward to adopt. Based on our first-hand experience 
following the convention, as well as teaching hands-on tutorials for 
the past year, we have identified two broad classes of users. On the 
one hand, there is the user that goes through the learning curve and 
experiences the benefits of following the convention, both at the 
personal level, and when collaborating with others. In words of some 
of the attendees to the tutorials, "it quickly pays off" to go through 
the learning process.

On the other hand, the fact that a Popper pipeline is implemented _a 
priori_ (i.e. before an article has written), and that creating 
reusable pipelines implies the use of new tools (such as Docker and 
Spack), its adoption is seen by some potential new users as a big 
paradigm shift. The main criticism from this set of people is that 
"there is no time" for a researcher or student to do things in this 
"radically new way".

Given the above, we see an opportunity to develop new technology to 
close this gap. The main objective is to create a platform where it is 
_ridiculously easy_ to both, implement and re-execute experimentation 
pipelines. Our target quantifiable goals are "push-button" 
repeatability (re-execute an existing experiment) and "no more than 10 
minutes" to assemble the skeletal components of a new scientific 
exploration pipeline (a few clicks on a web-based GUI should suffice).

[^devopstoolkit]: For our purposes, any tool that can be invoked on 
the CLI and can be given a script as input is a so-called "DevOps" 
tool. For more on other aspects of DevOps tools see 
<http://12factor.net/>.

[^popperized]: <https://popper.rtfd.io/en/latest/sections/examples.html>

# Black Swan {#sec:components}

# Use cases {#sec:cases}

We describe two use cases for which we envision Black Swan to be 
applicable. We envision many more but we see these as key use cases.

## Technology Transfer

Organizations with _Research and Development_ units such as tech 
companies and government-funded institutions (DOE labs, NASA, 
universities) spend significant amounts of resources transferring 
technology from R&D to production environments. Deploying an instance 
_Black Swan_ internally, using a public one (or connecting a public 
with a private one), would allow organizations to streamline tech (and 
knowledge) transfers.

## Research Curation

In the past decade, institutional libraries have invested a 
significant amount of resources to the creation of _Data 
Repositories_. These efforts are aimed at systematically managing the 
output of research. For example, EU's [OpenAIRE](http://openaire.eu/) 
initiative is a catalog of software and data containing more than 80 
million entries (each being a code repository or dataset). Similar 
efforts are underway in the US such as the [Center for Open Science's 
Open Science Framework](https://osf.io). We see _Black Swan_ 
complementing these efforts, since Popper enables the curation of 
research by enabling all these existing repositories to be easily 
executed and validated over time.

# Related Software {#sec:related}

There are mainly two types of existing software that relate to Black 
Swan.

  * **Continuous Integration (CI) Software**. Black Swan will leverage 
    CI services to execute and validate research pipelines. OSS 
    communities such as those developing and using Gitlab, Github and 
    Jenkins can benefit from this platform.

  * **Code and Data Repository Management Software**. OSS communities 
    developing code and data management repositories such as [DOE 
    Code](https://www.osti.gov/doecode/), [Zenodo](http://zenodo.org/) 
    and [OSF](http://osf.io/) can leverage Black Swan to add 
    executable features to the repositories that they curate.

# References {.unnumbered}

\noindent
\vspace{-1em}
\setlength{\parindent}{-0.175in}
\setlength{\leftskip}{0.2in}
\setlength{\parskip}{0.5pt}
\fontsize{7pt}{8pt}\selectfont
