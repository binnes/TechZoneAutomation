# Steps for iascable

## Setup and first deploy (podman)
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
     !!!Note:
     the launch script will attach a podman volume for the workspace filesystem, which persists across multiple container runs, so you may need to clear the workspace directory if you don’t need the content from previous runs.