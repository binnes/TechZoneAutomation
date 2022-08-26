## Setup and first deploy (multipass)[¶](https://binnes.github.io/TechZoneAutomation/setup/#setup-and-first-deploy-multipass)

!!!Warning:
Multipass networking doesn't work (no external connectivity, though name resolution works) with Cisco AnyConnect running! Turning off Cisco AnyConnect and the networking works - you cannot start Cisco AnyConnect while multipass is running or the network will be killed.

!!!Todo
    This section is under construction - need to split into sections covering install, creating BOM and then deploying BOM instead of single set of steps

Create or change into the directory containing your BOM then run the following commands:

1. install multipass :

   ```shell
   brew install --cask multipass
   ```
2. download the cloud init file :

   ```shell
   curl https://raw.githubusercontent.com/cloud-native-toolkit/sre-utilities/main/cloud-init/cli-tools.yaml --output cli-tools.yaml
   ```
3. launch multipass vm :

   ```shell
   multipass launch --name cli-tools --cloud-init ./cli-tools.yaml
   ```
4. mount current directory into VM : `

   ```shell
   multipass mount $PWD cli-tools:/automation
   ```
5. enter vm :

   ```shell
   multipass shell cli-tools
   ```
6. install iascable :

   ```shell
   curl -sL https://raw.githubusercontent.com/cloud-native-toolkit/iascable/main/install.sh | sudo bash
   ```

   - this is different to the command given in the docs (pipe into bash not sh)
7. create BOM e.g. my-ibm-vpc-gitops.yaml

   ```yaml
   apiVersion: cloudnativetoolkit.dev/v1alpha1
   kind: BillOfMaterial
   metadata:
     name: my-ibm-vpc-gitops
   spec:
     modules:
       - name: ibm-vpc
       - name: ibm-vpc-subnets
       - name: ibm-vpc-gateways
       - name: ibm-ocp-vpc
         variables:
           - name: worker_count
             value: 1
       - name: gitops-repo
       - name: argocd-bootstrap
   ```
8. run iascable build:

   ```shell
   iascable build -i oc-dev.yaml
   ```
9. run the terraform apply (optionally a variables.yaml file can be created - if not you will be prompted for required values) :

   ```shell
   cd output/my-ibm-vpc-gitops
   ./apply.sh
   ```
10. answer any prompts for missing variable values, check the steps listed and confirm the actions by responding **yes**
11. wait for terraform and gitops to complete the install

Issues:

- guidance on variable values to be provided is needed
- certain modules fail (sealed-secrets-controller image fails to pull from docker.io - timeout)

## Setup and first deploy (podman)[¶](https://binnes.github.io/TechZoneAutomation/setup/#setup-and-first-deploy-podman)

Create or change into the directory containing your BOM then run the following commands:

1. install podman :

   ```shell
   brew install podman
   ```
2. install iascable if not already installed :

   ```shell
   curl -sL https://raw.githubusercontent.com/cloud-native-toolkit/iascable/main/install.sh | sudo sh
   ```
3. initialise podman :

   ```shell
   podman machine init
   ```
4. enable podman root :

   ```shell
   podman machine set --rootful
   ```
5. start podman machine :

   ```shell
   podman machine start
   ```
6. create BOM e.g. my-ibm-vpc-gitops.yaml

   ```yaml
   apiVersion: cloudnativetoolkit.dev/v1alpha1
   kind: BillOfMaterial
   metadata:
     name: my-ibm-vpc-gitops
   spec:
     modules:
       - name: ibm-vpc
       - name: ibm-vpc-subnets
       - name: ibm-vpc-gateways
       - name: ibm-ocp-vpc
         variables:
           - name: worker_count
             value: 1
       - name: gitops-repo
       - name: argocd-bootstrap
   ```
7. run iascable build:

   ```shell
   iascable build -i my-ibm-vpc-gitops.yaml
   ```
8. launch the tools container :

   ```shell
   cd output 
   ./launch.sh podman --pull
   ```
9. copy the mounted directory to a container directory (needed as podman has issues with symbolic links on a mounted directory) :

   ```shell
   cp -R * /workspaces
   ```
10. run the terraform apply (optionally a variables.yaml file can be created - if not you will be prompted for required values) :

    ```shell
    cd /workspaces/my-ibm-vpc-gitops
    ./apply.sh
    ```
11. answer any prompts for missing variable values, check the steps listed and confirm the actions by responding **yes**
12. wait for terraform and gitops to complete the install

Note

the launch script will attach a podman volume for the workspace filesystem, which persists across multiple container runs, so you may need to clear the workspaces directory if you don't need the content from previous runs.

## Setup and first deploy (Docker)[¶](https://binnes.github.io/TechZoneAutomation/setup/#setup-and-first-deploy-podman)

Docker desktop should be installed and be running.

1. create BOM e.g. my-ibm-vpc-gitops.yaml

   ```yaml
   apiVersion: cloudnativetoolkit.dev/v1alpha1
   kind: BillOfMaterial
   metadata:
     name: my-ibm-vpc-gitops
   spec:
     modules:
       - name: ibm-vpc
       - name: ibm-vpc-subnets
       - name: ibm-vpc-gateways
       - name: ibm-ocp-vpc
         variables:
           - name: worker_count
             value: 1
       - name: gitops-repo
       - name: argocd-bootstrap
   ```
2. run iascable build:

   ```shell
   iascable build -i my-ibm-vpc-gitops.yaml
   ```
3. launch the tools container :

   ```shell
   cd output 
   ./launch.sh docker --pull
   ```
4. copy the mounted directory to a container directory :

   ```shell
   cp -R * /workspaces
   ```
5. run the terraform apply (optionally a variables.yaml file can be created - if not you will be prompted for required values) :

   ```shell
   cd /workspaces/my-ibm-vpc-gitops
   ./apply.sh
   ```
6. answer any prompts for missing variable values, check the steps listed and confirm the actions by responding **yes**
7. wait for terraform and gitops to complete the install
