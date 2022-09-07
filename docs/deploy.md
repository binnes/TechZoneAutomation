# Deploy Bill of Materials

When you have your [Bill of Materials (BOM)](bom.md){target=:blank} you can deploy it to create the desired environment.

!!!Todo
    what is the preferred/opinionated way to pass variables into the deploy process?

    -   in the BOM (is a BOM a template to apply to multiple environments, or an environment specific configuration?)
    -   a variables file - variables.yaml or directly modify terraform variable files
    -   environment variables
    -   how are sensitive credentials supposed to be managed?

## Variables

When a BOM is deployed there are some variables (values) that need to be provided to configure each module so the desired state can be achieved.

When a module is created, a set of input variables are defined with the option of providing a default value for an input variable.

Some of the input variables may be generated by a dependent module.

The module.yaml file for a module defines the required input variables and if the variable comes from a dependent module.  It is also possible for a module to define optional input variables.

As the modules are Terraform modules, there are also the Terraform variable definitions (usually in variables.tf and output.tf).  The Terraform files have the variable definitions along with any default values.

!!!Todo
    What is the relationship between the input/output variables defined in the module.yaml file and the Terraform input/output variables defined in the Terraform (.tf) files?

    -   is there any validation between the .yaml BOM and .tf files?
    -   what is the purpose of the variable information in the modules.yaml file?
        -   is it used to generate the Terraform files in the output folder?
    -   should the Terraform files be regarded as the source of truth?

You will need to provide some input variables for most modules.  These allow you to customise the deployment without needing to alter the module code.  It is possible to add variable values to the BOM yaml file, but Terraform also provides a number of ways to provide values to input variables:

-   using the **-var** command line argument to pass values when invoking Terraform on the command line or from a script
-   from a file containing the variable values
-   as environment variables.  The environment variable format Terraform uses is TF_VAR_ prepended to the input variable name

If the **launch.sh** script is used to launch a tools container (Docker or Podman), then there is a specific process in place to pass environment variables using a **credentials.properties** file, which will set the environment variables within the tools container.  The credentials file is also used to set the environment variables inside the **multipass** virtual machine.

The **credentials.properties** file contains lines of the format:

``` shell
export TF_VAR_variable_name="variable_value"
```

This format ensure the environment variables will be correctly set when using multipass or using containers.

!!!Note
    The **-var** command line argument to the **terraform** command can only be used if you are not using the **apply.sh** script.  See the [deploy](deploy.md) section for details of running a deployment

!!!Todo
    need a new version of iascable to be released to incorporate this [pull request](https://github.com/cloud-native-toolkit/iascable/pull/190).  Until then the apply.sh script will need to be manually modified after **iascable build** is run

    line 37 of apply.sh script should be `environment_variable=$(env | grep "${variable_name}" | sed -E 's/.*=(.*).*/\1/g')`

If variables are not passed into the deploy and no default value has been defined, then you will be prompted to provide the values at deploy time

## Deploying the Bill of Materials



!!!todo
    How to manage Terraform state in a shared, team environment?