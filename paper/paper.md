---
title: "Spotting Black Swans With Ease: The Case for a Practical Reproducibility Platform"
shorttitle: Black Swan
author: Ivo Jimenez and Carlos Maltzahn (UC Santa Cruz)
abstract: |
 Advances in agile software delivery methodologies and tools (commonly 
 referred to as _DevOps_) have not yet materialized in academic 
 scenarios such as university, industry and government laboratories. 
 In this position paper we make the case for _Black Swan_, a platform 
 for the agile implementation, maintenance and curation of 
 experimentation pipelines by embracing a DevOps approach.
---

# Introduction

Reproducibility is the cornerstone of the scientific method. Yet, in 
computational and data science domains, a gap exists between current 
practices and the ideal of having every new scientific discovery be 
_easily_ reproducible [@donoho_reproducible_2009]. Advances in 
computer science (CS) and software engineering slowly and painfully 
make their way into these domains---even in CS research itself
[@fursin_collective_2013 ; @collberg_repeatability_2016], paradoxically.

Popper [@jimenez_popper_2017] is an experimentation protocol and CLI 
tool for implementing scientific exploration pipelines following a 
DevOps approach. The goal of Popper is to bring the same methods and 
tools used for the agile delivery of software, known as DevOps 
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
between research and operational environments.

This paper is organized as follows. @Sec:motivation expands on the 
need for Black Swan, while @Sec:components presents the components of 
the proposed platform and how they address the current gaps. 
@Sec:cases illustrates the utility of the platform by describing use 
cases where Black Swan would be used. We close by discussing 
challenges in @Sec:challenges.

[^name]: Black swans are typically used to illustrate the concept of 
falsifiability: the statement "all swans are white" is made 
falsifiable by defining the condition under which it would be false, 
i.e. finding one or more non-white swans. In this case, the statement 
was actually proven false by the discovery of black swans in Australia 
in the 1600's. We envision _Black Swan_ (the platform) to be a tool 
for researchers to _easily_ find black swans in their computational or 
data science theories (i.e. identify when their claims are false) and, 
more importantly, allow them to investigate **why**.

# Motivation {#sec:motivation}

Following the Popper experimentation protocol turns out to be a 
significant paradigm shift for many users that is hard to accept in 
the context of tight submission for publication deadlines. It begins 
by defining a pipeline for a scientific exploration, i.e. a sequence 
of high-level steps that are carried out when executing an experiment 
or analysis. For example, a data analysis pipeline may consist of four 
stages: (1) obtain a dataset; (2) pre-process the data; (3) run an 
analysis on the data; and (4) produce plots.

The contents of a folder containing scripts for a pipeline are shown 
in @Lst:pipe (examples are available on [Github][popperized]). Each 
stage in a pipeline corresponds to a Bash script, and it codifies what 
a person would manually type in a terminal otherwise. A stage in a 
pipeline is a relatively simple list of steps, where each invokes 
other tools, and passes scripts to them, which are themselves stored 
in the same repository.

```{#lst:pipe caption="Contents of a pipeline."}
paper-repo/pipelines/gassyfs
|- README.md
|- baseliner
|  |- config.yml
|- docker
|  |- Dockerfile
|- geni
|  |- cloudlab_request.py
|- results
|  |- output.csv
|- run.sh
|- setup.sh
|- validate.sh
```

There are four key aspects that make Popper pipelines practical. (1) 
They are version-controlled, which allows readers to understand what 
was done over time (and why), mimicking a lab notebook; (2) thanks to 
virtualization technology at the language-, OS- or hardware-level, 
they are portable and can be easily re-executed in many environments; 
(3) are automated; and (4) are self-contained.

While implementing a Popper pipeline can be done in many ways 
[^devopstoolkit], in order for a pipeline to be _Popper Compliant_, it 
must be a _Self-verifiable Experimentation Pipeline_ (SEP) 
[@jimenez_selfverifiable_2018]. A SEP is a pipeline that carries out 
the following high-level tasks in every execution:

 1. **Code and data checkout**. Code must reside on a version control 
    system (e.g. [Github][gh], [Gitlab][gl], etc.). If datasets are 
    used, then they should reside in a dataset management system 
    ([Zenodo][zenodo], [CKAN][ckan], [Datapackages][datapackages], etc.). 
    The experimentation pipelines must obtain the code/data from these 
    services on every execution.
 2. **Setup**. The pipeline should build and deploy the code under 
    test. For example, if a pipeline is using containers or VMs to 
    package their code, the pipeline should build the container/VM 
    images prior to executing them. The goal of this is to verify that 
    all the code and 3rd party dependencies are available at the time 
    a pipeline runs, and that software can be build correctly.
 3. **Resource allocation**. If a pipeline requires a cluster or 
    custom hardware, resource allocation must be 
    done as part of the execution of the pipeline. This allocation can 
    be static or dynamic. For example, if an experiment runs on custom 
    hardware, the pipeline can statically allocate (i.e. hardcode 
    IP/hostnames) the machines where the code under study runs (e.g. 
    GPU/FPGA nodes). Alternatively, a pipeline can dynamically 
    allocate nodes on CloudLab [@ricci_introducing_2014], Grid500K 
    [@bolze_grid_2006], Chameleon [@mambretti_next_2015], or a public 
    cloud provider. These services typically offer infrastructure 
    automation tools such as [Geni-lib][genilib] (Cloudlab), Enos 
    [@cherrueau_holistic_2017] (Chameleon), SLURM [@yoo_slurm_2003] 
    (HPC centers), or [Terraform][terraform] (AWS, GCP, etc.). All 
    these tools can be used to automate infrastructure-related tasks.
 4. **Explicit parametrization**. Parameters that are relevant to the 
    outcome of the pipeline need to be made explicit, for example by 
    creating a `parameters.yml` file.
 5. **Environment capture**. Capture information about the runtime 
    environment. For example, hardware description, OS, system 
    packages (i.e. software installed by system administrators), 
    information about remote services (e.g. version and state of a 
    batch scheduler), etc. Open-source tools such as 
    [SOSReport][sosreport] or [Facter][facter] can aid in aggregating 
    this information.
 6. **Results Validation**. Scripts must verify that the output 
    corroborates the claims made on the article. For example, the 
    pipeline might check that the throughput of a system is within an 
    expected confidence interval (e.g. defined with respect to a 
    baseline obtained at runtime), or that a numerical computation is 
    within some expected bounds. This can be done with domain-specific 
    tools [@jimenez_aver_2016], or generic data analytics stacks such 
    as Python (Pandas [@mckinney_data_2010]) or R 
    [@r.coreteam_language_2013].

Since a Popper pipeline is, fundamentally, a list of Bash scripts 
(with a specific execution order) stored in a version control 
repository with a pre-defined folder structure, implementing SEPs is 
an arguably straightforward habit to adopt [^argueable]. However, 
based on our first-hand experience following the protocol in our 
laboratory, as well as teaching hands-on tutorials for the past year, 
we have identified two broad classes of users. On the one hand, there 
is the user that goes through the learning curve and experiences the 
benefits of following the protocol, both at the personal level, and 
when collaborating with others. In words of some of the attendees to 
the tutorials, "it quickly pays off" to go through the learning 
process. On the other hand, the fact that a Popper pipeline is 
implemented _a priori_ (i.e. before an article has written), and that 
creating reusable pipelines implies the use of new tools (such as 
Docker or Spack), its adoption is seen by some potential new users as 
a big paradigm shift. The main criticism from this set of people is 
that "there is no time" for a researcher or student to do things in 
this "radically new way".

Given the above, we see an opportunity to develop new OSS technology to 
close the gap for those that still resist to take the leap. The main 
objective is to create a platform where it is _ridiculously easy_ to 
both, implement and re-execute experimentation pipelines. Our target 
quantifiable goals are "push-button" repeatability (re-execute an 
existing experiment) and "no more than 10 minutes" to assemble the 
skeletal components of a new scientific exploration pipeline (a few 
clicks on a web-based GUI should suffice).

[^devopstoolkit]: For our purposes, any tool that can be invoked on 
the CLI and can be given a script as input is a so-called "DevOps" 
tool. For more on other aspects of DevOps tools see [@kim_devops_2016] 
and `http://12factor.net`.

[^argueable]: Based on our first-hand experience, we spend at most the 
same amount of time implementing a pipeline; what changes is the 
approach, i.e. we write scripts to automate a pipeline instead of 
manually executing it. This, in turn, means that the more we 
re-execute a pipeline, and the more we reuse stages across distinct 
explorations, the more it pays off.

[popperized]: <https://popper.rtfd.io/en/latest/sections/examples.html>
[gh]: <https://github.com>
[gl]: <https://gitlab.com>
[zenodo]: <https://zenodo.org>
[ckan]: <https://ckan.org/>
[datapackages]: <https://frictionlessdata.io/data-packages/>
[genilib]: <https://bitbucket.org/barnstorm/geni-lib>
[sosreport]: <https://github.com/sosreport/sos>
[facter]: <https://github.com/puppetlabs/facter>
[terraform]: <https://terraform.io>

# Black Swan {#sec:components}

Once implemented, Black Swan will be a community-driven 
reproducibility platform that enables the agile delivery and 
validation of scientific insights. There are five main functional 
components of the platform.

**External Service Integrations**

One design principle is not to re-invent the wheel. That is, rather 
than re-implementing functionality found in other services or tools, 
Black Swan will have a pluggable mechanism so that it can integrate 
with these. A diagram of basic integrations is shown in 
@Fig:integrations. Version control services will store a pipeline's 
content; input and output will be version-controlled by connecting to 
dataset management services; CI services will continuously validate 
the integrity of a pipeline; and a database service will be used to 
store the history of executions for each pipeline, and as much 
environmental information as possible.

![Black Swan will leverage existing services.
](figures/integrations.png){#fig:integrations}

**Automated Compliance Verification**

Black Swan automates the process of verifying that a pipeline complies 
with the SEP criteria outlined previously, in a domain-agnostic way. 
Verification is done at runtime and is based on conventions of what 
the output of a pipeline looks like. For example, to verify that code 
and data repositories are being cloned, the output of a pipeline 
(what's get printed to `stdout`) is inspected to verify that 
repositories are being contacted (e.g. downloading from Github or 
Zenodo); the task of infrastructure allocation is expected to generate 
a `manifest.yml` (or `system.json`) file; explicit parametrization is 
verified by checking whether a `parameters.yml` file exists; and so on 
and so forth. By default, a pre-defined list of checks will be 
supported but the underlying mechanism will be extensible so more 
items are added in order to verify SEP compliance.

**Pipeline Catalog and Pipeline Builder**

In order to facilitate the adoption of the DevOps practice, the 
platform will incorporate a GUI component to allow users to visualize 
Popper pipelines and their stages. In particular, a _Pipeline Builder_ 
will allow users to "mix and match" stages from an existing catalog 
of SEP compliant, community-maintained pipelines (@Fig:pipecatalog). 
The reusable catalog and the pipeline builder illustrate the 
importance that community will have in the success of Black Swan as an 
OSS project. Unless there is a community-wide effort in place, Black 
Swan will not succeed as a viable project; maintaining a pipeline by a 
single individual is too overwhelming.

![Sketch of the pipeline builder GUI.
](figures/pipecatalog.pdf){#fig:pipecatalog}

**Differential Analysis**

Whenever the validation stage of a SEP pipeline fails, we have found a 
black swan, i.e. the domain-specific expectations on results do 
not hold. Having access to the environmental information on which 
hardware and software was a pipeline re-executed on, Black Swan will 
provide facilities to automatically analyze and compare against 
previous successful executions in order to determine root causes of 
irreproducibility, or to aid researchers in finding them by 
incorporating GUI elements to support the visual inspection of 
differences.

**Validation Dashboard**

Borrowing ideas from [@jimenez_popperci_2017], Black Swan will 
incorporate facilities to quickly visualize the status of a pipeline 
with respect to domain-specific validations. For failed validations, 
the dashboard will provide links to trigger differential analysis with 
respect to previous successful executions, as described above.

# Use cases {#sec:cases}

We describe three use cases for which we envision Black Swan to be 
directly applicable.

**Technology Transfer**

Organizations with Research and Development (R&D) units such as tech 
companies and government-funded institutions (DOE labs, NASA, 
universities) spend significant amounts of resources transferring 
technology from R&D to production environments. Deploying an instance 
_Black Swan_ internally (or using a public one) would allow 
organizations to streamline tech (and knowledge) transfers.

**Research Curation**

In the past decade, institutional libraries have invested a 
significant amount of resources in the creation of _Data 
Repositories_. These efforts are aimed at systematically managing the 
output of research. For example, EU's [OpenAIRE](http://openaire.eu/) 
initiative is a catalog of software and data containing more than 80 
million entries (each being a code repository or dataset). Similar 
efforts are underway in the US such as the [Center for Open Science's 
Open Science Framework](https://osf.io). We see _Black Swan_ 
complementing these efforts, since Popper enables the curation of 
research by enabling all these existing repositories to be easily 
executed and validated over time.

**Self-validation of Academic Articles**

A growing number of Computer Science conferences and journals 
incorporate an artifact evaluation process in which authors of an 
article submit an artifact description[^ad] (AD) that is tested by a 
committee, in order to verify that experiments presented in a paper 
can be re-executed by others. Black Swan can be leveraged to 
automatically test the reproducibility aspects of articles, assuming 
the pipeline(s) corresponding to an article are SEP compliant 
[@jimenez_selfverifiable_2018].

[^ad]: http://ctuning.org/ae/submission.html

# Challenges {#sec:challenges}

Managing changes to code using version-control systems; managing data 
with dataset management systems; continuously integrating (CI) and 
deploying (CD) software; all have become standard practices in OSS 
communities, not because of a fad but because of the quantifiable 
benefits that following best practices bring. To the contrary, in R&D 
settings, these practices are seen as a burden. The biggest challenge 
we face lies in changing the culture within organizations and teams; 
finding the right incentives so that these users can make the leap and 
adopt agile practices, so they can enjoy the benefits that come from 
embracing DevOps.

# References {.unnumbered}

\noindent
\vspace{-1em}
\setlength{\parindent}{-0.175in}
\setlength{\leftskip}{0.2in}
\setlength{\parskip}{0.5pt}
\fontsize{7pt}{8pt}\selectfont
