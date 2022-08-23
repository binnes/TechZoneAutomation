# Planning

The following investigation needs to be done and written up in preparation for creating user documentation.

The work will be split into 2 sections:

1. Consumption - how to use TechZone Automation to create an OpenShift Cluster on different clouds then install and configure a defined set of software
2. Contribute - how to contribute modules to TechZone Automation to extend the catalog of modules available to install.  This may include open source software, other commercial software offerings

## Consumption

Before someone can contribute a module they need to understand how TechZone Automation works and how to use it to lay down a cluster and set of software. The investigations in this section should:

### Setup

-   cover system setup to be able to use TechZone Automation on:
    -   MacOS (Intel)
    -   MacOS (Apple Silicon)
    -   Windows (10 and 11)
    -   Linux (Fedora)
    -   Linux (Ubuntu)

    In addition to the OS options there should be clear instructions to cover the following container tooling options:

    -   Docker
    -   Podman (if this is going to be a supported option going forward?)
    -   Multipass

### Building BOM

There is an extensive [catalog of modules](https://modules.cloudnativetoolkit.dev){target=:blank} so some explanation is needed to cover the tooling available and also explain how modules work to build an entire system (dependencies, variables, etc....)

### Installing an environment using a BOM

How to take a BOM and *run it* to create an environment (this should work with all allowed OS and container tool combinations)

## Contribution

!!!Todo
    Complete this part of the plan
