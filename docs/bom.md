# Bill of Materials

To deploy infrastructure or software using TechZone Accelerator you need to start by creating a **Bill of Materials** (BOM) or using a predefined BOM.  The [TechZone Accelerator Toolkit](https://builder.cloudnativetoolkit.dev){target=:blank} site is the starting place to see the modules catalog and list of pre-defined solutions.

The BOM defines the modules you want to install.  Available modules can be found in the [Module Catalog](https://modules.cloudnativetoolkit.dev){target=:blank}

See the [operate documentation](https://operate.cloudnativetoolkit.dev/apply/bill-of-material-reference/){target=:blamk} for additional details of the BOM content

!!!Todo
    Why are some modules not in the catalog (e.g. [k8s-ocp-cluster](https://github.com/cloud-native-toolkit/terraform-k8s-ocp-cluster) - does this mean they are obsolete or incomplete?

## Dependencies

When you want to install a module there maybe some dependencies that module needs to allow it to be installed.  For example, if you want to use GitOps (ArgoCD) to install an application then GitOps needs to be available and configured on the target cluster.  Similarly if you want to install and configure GitOps then I need to have a cluster to install GitOps into.

Every module in then TechZone Accelerator Toolkit defines it's dependencies in a module.yaml file in the module's github repository.  The module's repo is linked by clicking on the module name in the [modules catalog](https://modules.cloudnativetoolkit.dev){target=:blank}

### Alias

Sometimes a module dependency can be satisfied by multiple modules.  In this case an alias can be used, where a module can register that it satisfies an alias.  An example of this is the alias **cluster**.  Any module that makes a Kubernetes cluster available to other modules will define the **cluster** alias.  This then provides a generic way to specify a cluster dependency.  So long as a module is included in a BOM that satisfies the cluster alias the dependency will be met.

If you explore the **cluster** category in the [modules catalog](https://modules.cloudnativetoolkit.dev){target=:blank} you will see a number of options for installing a Kubernetes cluster, including an option to simple provide the login to an existing cluster - all these modules will satisfy the **cluster** dependency.

### Optional

Some dependencies can be specified as optional.  This is where a module may be able to complete the installation without the optional dependency being satisfied in the BOM.  An example of this is that modules using GitOps to perform an installation will specify the cluster as optional.  This is because the module doesn't need to interact with the cluster to complete the installation.  The module can simple create content in a GitOps repository to complete the installation on a cluster.

### Outstanding Questions

!!!Todo
    -   In the module.yaml what are the **refs** and **interface** properties used for?
        -   What does it mean when a dependency has multiple refs (e.g. [artifactory](https://github.com/cloud-native-toolkit/terraform-tools-artifactory/blob/main/module.yaml))?
    -   How does the **platform** property work - any implications/restrictions or is this a testing statement or is there logic in the toolkit tool which checks all modules support the deployed platform?

    How are dependencies resolved? 
    
    -   if a single module satisfies a dependency is it automatically selected?  
    -   can a default module be specified if there are multiple modules that satisfy a dependency and one is not included in a BOM?
    -   is there a feature to allow an OR with dependencies (e.g. I need to have a MySQL or Postgres DB)?

### Verifying all dependencies are resolved

Once a BOM has been completed you can validate that all dependencies have been satisfied or can be resolved by running the **iascable** tool.  This will be covered in more detail in the [deploy](deploy.md) section.

``` shell
    iascable build -i my_bom.yaml
```

*where **my_bom.yaml** is the file containing the BOM you want to verify*

The iascable build command will create a folder called output, if it doesn't already exist, then generate a folder within the output folder named using the **name** property in the metadata section of the BOM.  

Within this folder will be a file called **bom.yaml** which will be an expanded version of the original BOM (my_bom.yaml), pulling in all dependencies.  If a dependency cannot be automatically resolved you will get an error detailing which dependencies cannot be resolved.

## Variables

!!! Todo
    Important flag in modules and BOM will cause TFVARS template to be generated
