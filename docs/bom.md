# Bill of Materials

To deploy infrastructure or software using TechZone Automation you need to start by creating a **Bill of Materials** (BOM)  or using a predefined BOM.

!!!Todo
    Link the ascent tool and the predefined BOMs

The BOM defines the modules you want to install.  Available modules can be found in the [Module Catalog](https://modules.cloudnativetoolkit.dev){target=:blank}

## Investigations to do

-   dependencies (including default vs specified), aliases and dependency resolution (*iascable build*)
-   variables, input and output
-   how to discover what a module does

## Dependencies

When you want to install a module there maybe some dependencies that module needs to allow it to be installed.  For example, if you want to use GitOps (ArgoCD) to install an application then GitOps needs to be available and configured on the target cluster.  Similarly if I want to install and configure GitOps then I need to have a cluster to install GitOps into.

Every module in TechZone automation defines it's dependencies in a module.yaml file in the module's github repository.  Thr module's repo is linked by clicking on the module name in the [modules catalog](https://modules.cloudnativetoolkit.dev){target=:blank}

### Alias

Sometimes a module dependency can be satisfied by multiple modules.  In this case an alias can be used, where a module can register that it satisfies an alias.  An example of this is the alias cluster.  Any module that makes a Kubernetes cluster available to other modules will define the **cluster** alias.  This then provides a generic way to specify a cluster dependency.  So long as a module is included in a BOM that satisfies the cluster alias the dependency will be met.

If you explore the **cluster** category in the [modules catalog](https://modules.cloudnativetoolkit.dev){target=:blank} you will see a number of options for installing a Kubernetes cluster, including an option to simple provide the login to an existing cluster - all these modules will satisfy the **cluster** dependency.

### Optional

Some dependencies can be specified as optional.  This is where a module may be able to complete the installation without the optional dependency being satisfied in the BOM.  An example of this is that modules using GitOps to perform an installation will specify the cluster as optional.  This is because the module doesn't need to interact with the cluster to complete the installation.  The module can simple create content in a GitOps repository to complete the installation on a cluster.

!!!Todo
    In the module.yaml what are the **refs** and **interface** properties used for?  How does the **platform** property work - any implications/restrictions or is this a testing statement?

    How are dependencies resolved? 
    
    -   if a single module satisfies a dependency is it automatically selected?  
    -   can a default module be specified if there are multiple modules that satisfy a dependency and one is not included in a BOM?
