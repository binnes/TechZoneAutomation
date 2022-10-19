# Learning Environment

To create a zero-cost learning experience we want to use a local, low resource vanilla Kubernetes cluster using [minikube](https://minikube.sigs.k8s.io/docs/){target=_blank}

=== "multipass on mac"

    ## Multipass environment on Mac

    Create the cloud-init based on the Toolkit init file, call it **cli-tools-minikube.yaml**:

    ``` yaml
    ## cloud-init to create a VM with the following:
    ##
    ## - terraform
    ## - terragrunt
    ## - git
    ## - jq
    ## - yq
    ## - oc
    ## - kubectl
    ## - helm
    ## - ibmcloud cli

    apt:
      sources:
        helm.list:
          source: "deb https://baltocdn.com/helm/stable/debian/ all main"
          key: |
            -----BEGIN PGP PUBLIC KEY BLOCK-----

            mQINBF6yP7IBEADWk4aijQ7Vhj7wn2oz+8asnfzsD0+257qjWy1m+cN4RP6T2NBG
            S2M5+vzbsKNmGAja8jOpo46pHo/SCdc8Bwv+QHH+JbuBbDNEHwIBGV5p+ZRETiHq
            l8UsyUAPCWinKR6evZrANCBEzXtOEVJ4thuPoBuZkteKNTdPlOg9MBqD5zz+4iQX
            2CJJNW7+1sxAAVozHJxjJbu6c84yPvNFAiCAct+x5WJZFJWuO+l55vl6va8cV7tw
            DgHomk+1Q7w00Z0gh28Pe1yfvvw3N+pCSYn88mSgZtdP3wz3pABkMe4wMobNWuyX
            bIjGMuFDs7vGBY6UCL6alI/VC7rrSZqJZjntuoNI0Xlfc3BjUHWzinlbA7UFk5Lv
            qZO61V439Wm4x2n1V+4Kj/nPwtgBrNghaeDjxWLlgqaqynltSpXYnv2qGWYLRUb9
            WFymbYCJ0piqRdNVNNI8Ht9nFaya6qjDcIxFwFMF9QcrECG1HCK1M5JjdJpzr6Jq
            Z27/2ZG7DhabArSR5aoyBqhCylJfXugneDhitmilJiQd5EzefjiDO29EuBSMwkAs
            +IKg9jxGyI47m3+ITWhMDWTFTYBF/O69iKXfFvf4zrbfGMcf2w8vIOEBU3eTSYoY
            RhHXROedwcYpaVGJmsaT38QTSMqWTn12zlvmW5f6mEI5LQq398gN9eiWDwARAQAB
            tERIZWxtIGhvc3RlZCBieSBCYWx0byAoUmVwb3NpdG9yeSBzaWduaW5nKSA8Z3Bn
            c2VjdXJpdHlAZ2V0YmFsdG8uY29tPokCVAQTAQoAPhYhBIG/gy4vGc0qoEcZWSlK
            xIJ8GhaKBQJesj+yAhsvBQkSzAMABQsJCAcCBhUKCQgLAgQWAgMBAh4BAheAAAoJ
            EClKxIJ8GhaKPHEP/RRzvYCetoLeIj5FtedbeumGcWaJj97L4R1j7iK0dc0uvg0T
            5JeMDttAt69dFPHyB0kR1BLSwgJBhYCtvwalvD/g7DmL5l5HIM7o/VrkXDay1Pee
            wkCclA18y2wNM5EXKAuoFX5FMkRpTtSQhMMllbKsNNSvwvEZWvqMQlwJ/2HgNoVl
            2NtfY65UXHvIV2nTTmCVDq4OYBlHoUX5rRE7fOgFZ+u6Su7yopTYy13yY8ZVDNf/
            qNUWqA41gRYnwYtSq1DogHq1dcyr/SW/pFsn4n4LjG+38CIkSjFKOeusg2KPybZx
            l/z0/l0Yv4pTaa91rh1hGWqhvYDbLr2XqvI1wpcsIRPpU8lasycyQ8EeI4B5FVel
            ea2Z6rvGtMG92wVNCZ6YMYzpvRA9iRgve4J4ztlCwr0Tm78vY/vZfU5jkPW1VOXJ
            6nW/RJuc2mecuj8YpJtioNVPbfxE/CjCCnGEnqn511ZYqKGd+BctqoFlWeSihHst
            tuSqJoqjOmt75MuN6zUJ0s3Ao+tzCmYkQzn2LUwnYisioyTW4gMtlh/wsU6Rmims
            s5doyG2Mcc0QfstXLMthVkrBpbW4XT+Q6aTGUMlMv1BhKycDUmewI2AMNth5Hood
            iEt18+X26+Q2exojaMHOCdkUJ+C44XPDy6EvG4RyO4bILHz5obD/9QZO/lzK
            =BFdd
            -----END PGP PUBLIC KEY BLOCK-----

    packages:
      - git
      - jq
      - helm
      - unzip
      - openvpn
      - ca-certificates
      - graphviz
      - ubuntu-desktop
      - xrdp
      - firefox
    groups:
    - docker
    snap:
      commands:
      - [install, docker]
      - [install, kubectl, --classic]
    runcmd:
      - adduser ubuntu docker
      - mkdir -p /run/tmp
      - export TERRAFORM_VERSION=1.2.4 && curl -Lso /run/tmp/terraform.zip https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_$(if [ `uname -m` = arm64 -o `uname -m` = aarch64 ]; then echo "arm64"; else echo "amd64"; fi).zip && mkdir -p /run/tmp/terraform && cd /run/tmp/terraform && unzip /run/tmp/terraform.zip && sudo mv ./terraform /usr/local/bin && cd - && rm -rf /run/tmp/terraform && rm /run/tmp/terraform.zip
      - export TERRAGRUNT_VERSION=0.36.10 && curl -sLo /run/tmp/terragrunt https://github.com/gruntwork-io/terragrunt/releases/download/v${TERRAGRUNT_VERSION}/terragrunt_linux_$(if [ `uname -m` = arm64 -o `uname -m` = aarch64 ]; then echo "arm64"; else echo "amd64"; fi) && chmod +x /run/tmp/terragrunt && sudo mv /run/tmp/terragrunt /usr/local/bin/terragrunt
      - export YQ_VERSION=4.25.2 && curl -Lso /run/tmp/yq "https://github.com/mikefarah/yq/releases/download/v${YQ_VERSION}/yq_linux_$(if [ `uname -m` = arm64 -o `uname -m` = aarch64 ]; then echo "arm64"; else echo "amd64"; fi)" && chmod +x /run/tmp/yq && sudo mv /run/tmp/yq /usr/local/bin/yq
      - curl -Lo /run/tmp/kubectl "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/$(if [ `uname -m` = arm64 -o `uname -m` = aarch64 ]; then echo "arm64"; else echo "amd64"; fi)/kubectl" && chmod +x /run/tmp/kubectl && sudo mv /run/tmp/kubectl /usr/local/bin
      - export OPENSHIFT_CLI_VERSION=4.10 && sudo curl -Lo /usr/local/oc-client.tar.gz https://mirror.openshift.com/pub/openshift-v4/$(if [ `uname -m` = arm64 -o `uname -m` = aarch64 ]; then echo "arm64"; else echo "amd64"; fi)/clients/ocp/stable-${OPENSHIFT_CLI_VERSION}/openshift-client-linux.tar.gz && sudo mkdir /usr/local/oc-client && cd /usr/local/oc-client && tar xzf /usr/local/oc-client.tar.gz && sudo mv ./oc /usr/local/bin && cd - && sudo rm -rf /usr/local/oc-client && sudo rm /usr/local/oc-client.tar.gz
      - curl -fsSL https://clis.cloud.ibm.com/install/linux | sh && ibmcloud plugin install container-service -f && ibmcloud plugin install container-registry -f && ibmcloud plugin install observe-service -f && ibmcloud plugin install vpc-infrastructure -f && ibmcloud config --check-version=false
      - curl -sL https://iascable.cloudnativetoolkit.dev/install.sh | sh
      - curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-$(if [ `uname -m` = arm64 -o `uname -m` = aarch64 ]; then echo "arm64"; else echo "amd64"; fi)
      - install minikube-linux-$(if [ `uname -m` = arm64 -o `uname -m` = aarch64 ]; then echo "arm64"; else echo "amd64"; fi) /usr/local/bin/minikube
    ```

    The follow the steps below:

    1. launch the Multipass environment:

        ``` shell
        multipass launch --name cli-tools --cloud-init ./cli-tools-minikube.yaml --disk 40G --mem 4G --cpus 4 --timeout 480
        ```

    2. mount the current directory into the multipass environment:

        ``` shell
        multipass mount $PWD cli-tools:/automation
        ```

    3. enter the Multipass environment:

        ``` shell
        multipass shell cli-tools
        ```

    4. create the kubernetes environment:

        ``` shell
        minikube start --driver=docker --addons=dashboard,dns,ingress
        ```

=== "Podman on mac"

    ## Podman on Mac

    1. start podman machine

        ``` shell
        podman machine init --cpus=4 --memory=6096 -v $HOME:$HOME --rootful --now
        ```

        You need to know details of the podman network to get minikube working:

        ``` shell
        podman network ls
        ```

        my output looks like this:

        ``` text
        NETWORK ID    NAME        DRIVER
        2f259bab93aa  podman      bridge
        ```

        note the name of the network - **podman** then get details of the network with the `podman network inspace <network name>` command:

        ``` shell
        podman network inspect podman
        ```

        which generated the following output:

        ``` text
        [
            {
                  "name": "podman",
                  "id": "2f259bab93aaaaa2542ba43ef33eb990d0999ee1b9924b557b7be53c0b7a1bb9",
                  "driver": "bridge",
                  "network_interface": "podman0",
                  "created": "2022-10-19T16:57:22.965684025+01:00",
                  "subnets": [
                      {
                            "subnet": "10.88.0.0/16",
                            "gateway": "10.88.0.1"
                      }
                  ],
                  "ipv6_enabled": false,
                  "internal": false,
                  "dns_enabled": false,
                  "ipam_options": {
                      "driver": "host-local"
                  }
            }
        ]        
        ```

        the gateway address is needed when starting minikube - **10.88.0.1** in the example shown above. 

    2. install minikube

        !!! Todo
            add instructions

    3. create the kubernetes environment:

        ``` shell
        minikube start --driver=podman --apiserver-ips=127.0.0.1,10.88.0.1 --addons=dashboard,dns,ingress
        ```

        !!! note 
            change 10.88.0.1 in the above command to the gateway address of your podman network - found in step 1

        Once minikube is started you need to find what port the api is being served on.  To do this enter command

        ``` shell
        podman ps
        ```

        this will generate output similar to

        ``` text
        CONTAINER ID  IMAGE                                              COMMAND     CREATED         STATUS             PORTS                                                                                                                       NAMES
        d3a7f23e854b  gcr.io/k8s-minikube/kicbase:v0.0.34                            10 minutes ago  Up 10 minutes ago  0.0.0.0:32865->22/tcp, 0.0.0.0:40515->2376/tcp, 0.0.0.0:35427->5000/tcp, 0.0.0.0:39641->8443/tcp, 0.0.0.0:33853->32443/tcp  minikube
        ```

        look for the local port being mapped to container port 8443 : **0.0.0.0:39641->8443/tcp** in the example above

        within podman containers the minikube cluster api can be accessed at **`https://10.88.0.1:39641`**.  Note down the port number on your system as you will need it in step 11.

    4. expose dashboard via the ingress:

        ``` shell
        cat <<EOF | kubectl create -f -
        apiVersion: networking.k8s.io/v1
        kind: Ingress
        metadata:
          name: dashboard-ingress
          namespace: kubernetes-dashboard
        spec:
          rules:
          - host: dashboard.info
            http:
              paths:
                - pathType: Prefix
                  path: "/"
                  backend:
                    service:
                      name: kubernetes-dashboard
                      port:
                        number: 80
        EOF
        ```
    5. add name resolution for dashboard by adding `127.0.0.1  dashboard.info` to /etc/hosts file.  Choose your preferred editor, but you will need to edit the file using sudo

        ``` shell
        sudo nano /etc/hosts
        ```

        add the line `127.0.0.1  dashboard.info` to the bottom of the file, then **Ctrl-o** to write the file then **Ctrl-x** to quit the editor

    6. expose the ingress outside the minikube container

        ``` shell
        minikube tunnel
        ```

        enter your password when prompted, then press and hold the **Ctrl** key then press **z**.  The prompt should return then enter `bg` and press return.  Your dashboard should now be available
    
    7. access the dashboard at `http://dashboard.info`
    8. create/navigate to a directory where you want to work using the `cd` command.  This must be within your home directory for podman containers to be able to access the directory.  This will be referred to as the project directory (*e.g. ~/project/minikube*)
    9. create your first bill of materials

        ``` shell
        cat <<EOF > tutorial-bom.yaml
        apiVersion: cloud.ibm.com/v1alpha1
        kind: BillOfMaterial
        metadata:
            name: minikube-test
        spec:
            modules:
                - name: cluster-login
        EOF
        ```
    10. build the bill of material using command 

        !!! info "Temporary workaround"
            The cluster login module is not yet published, so need to add it manually so iascable can find it

            place the terraform-cluster-login.tgz file in the same folder as the tutorial-bom.yaml file then run the following command

            ``` shell
            tar zxvf terraform-cluster-login.tgz
            ```

            instead of the iascable command below this box use this command

            ``` shell
            iascable build -i tutorial-bom.yaml -c file:${PWD}/terraform-cluster-login/output/cluster-login/index.yaml
            ```

            then make the terraform-cluster-login.tgz file available within the tools container by moving it into the output directory

            ``` shell
            mv terraform-cluster-login.tgz output
            ```

            **DON"T RUN THE COMMAND BELOW - THAT IS FOR WHEN THIS WORKAROUND CAN BE REMOVED** move to step 11

        ``` shell
        iascable build -i tutorial-bom.yaml
        ```

    11. create the configuration needed to log into the cluster.  Change the **server_url** to the minikube api server address you discovered in step 3

        ``` shell
        cat <<EOF > tutorial/variables.yaml
        variables:
          - name: server_url
            value: https://10.88.0.1:39641
          - name: cluster_login_user
            value: minikube
          - name: cluster_ca_cert
            value: $(base64 -i ~/.minikube/ca.crt)
          - name: cluster_user_cert
            value: $(base64 -i ~/.minikube/profiles/minikube/client.crt)
          - name: cluster_user_key
            value: $(base64 -i ~/.minikube/profiles/minikube/client.key)
        EOF
        ```
    12. change to the output directory and launch into the cli-tools container

        ``` shell
        cd output
        ./launch.sh podman
        ```

        !!! info "Temporary workaround"
            The cluster login module is not yet published, so need to add it manually into the correct place for it to be found when the Bill of Materials is applied

            ``` shell
            sudo mkdir /automation
            cd /automation
            sudo tar zxvf /terraform/terraform-cluster-login.tgz
            ```

    13. apply the bill of materials

        ``` shell
        cd tutorial
        ./apply.sh
        ```

    14. set the kube config file to be able to access minikube within the cli-tools container

        ``` shell
        export KUBECONFIG=/terraform/tutorial/terraform/.tmp/.kube/config
        ```
