---
title: "Spotting Black Swans With Ease: The Case for a Practial Reproducibility Platform"
shorttitle: Black Swan
author: Ivo Jimenez and Carlos Maltzahn (UC Santa Cruz)
abstract: |
  We make the case for _Black Swan_, a platform for implementing, maintaining 
  and curating experimentation pipelines.
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
use of Popper in multiple domains has allowed us to identify opportunities 
where open-source software (OSS) can be used to close the existing gap 
in current research practices. While the Popper protocol is relatively easy to follow (wrap experimentation pipelines in the form of a sequence of Bash scripts), for many practitioners it still represents a big leap between current practices and where OSS development communities are (automated, portable and versioned pipelines). To this 
end, in this position paper we make the case for _Black Swan_[^name], 
a platform for _practical_ reproducible research. In a nutshell, Black 
Swan enables the agile delivery of software created in universities 
and other research institutions, significantly accelerating technology 
transfers between research and operation.

This position paper is organized as follows. Next section 
([@Sec:motivation]) expands on the need for _Black Swan_, while 
section [@Sec:components] presents the components of the proposed 
platform and how they address the current gaps. Section [@sec:cases] 
illustrates the utility of the platform by describing use cases where 
Black Swan would be used. We close with related work and how Black 
Swan is different to other existing software ([@sec:related]).

[^name]: Black swans are typically used to illustrate the concept of 
falsifiability: the statement "all swans are white" is proven false 
whenever one can spot one or more black swans. We envision _Black 
Swan_ (the platform) to be a tool for researchers to _easily_ find 
black swans in their computational or data science theories (i.e. 
identify when their claims are false) and, more importantly, allow 
them to investigate **why**.

# Motivation {#sec:motivation}

To illustrate the "gap"

  * popper pipelines.
  * they're easy but represent a big paradigm shift (and cognitive 
    burden).
  * we illustrate with an example.

```{#lst:gassyfs-dir .bash caption="Contents of the GassyFS 
experiment."}
paper-repo/experiments/gassyfs
├── README.md
├── baseliner
│   └── config.yml
├── docker
│   ├── Dockerfile
│   └── entrypoint.sh
├── geni
│   └── cloudlab_request.py
├── results
│   ├── output.csv
│   └── visualize.ipynb
├── run.sh
├── setup.sh
└── validate.sh
```

We have anecdotally shown (in our lab and from testimonies from 
"converts") how Popper is agile but for personal use and teams. 
However, from the point of view of "outsiders", it is still "too much 
work". Thus, we need to make it even easier, where the baseline is "I 
will just give it 2 minutes and then I'll give up", which in practice 
means just having time to create accounts on services and having 
budget for a few clicks.

# Black Swan {#sec:components}

Key Components:

  * Pipeline catalog
  * Pipeline builder
  * Environment capture

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

**Users**: Industry and university researchers; software engineers.

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

**Users**: Librarians; university and industry researchers.

# Related Work {#sec:related}

We present how it is different to existing software.

# References {.unnumbered}

\noindent
\vspace{-1em}
\setlength{\parindent}{-0.175in}
\setlength{\leftskip}{0.2in}
\setlength{\parskip}{0.5pt}
\fontsize{7pt}{8pt}\selectfont
