# Graduation demo

This Jenkinsfile contains a pipeline for CI/CD for a microservice application.

## Stages
| Stage                   | Description                                        |
|-------------------------|----------------------------------------------------|
| `git_clone`             | Clone git repository with microservices.           |
| `linter_tests `         | Run linter tests for such programming languages as: **Java**, **Go**, **Python** and **JavaScript**.  All tests that raise errors are wrapped in *catchError* blocks.|
| `create_docker_images`  | Collects all *docker images*, gives them a tag (hash of a commit) and sends it to **Google Registry**. |
| `deploy_to_k8s`         | Changes the tag (hash of the commit) of all *docker images*. Selects a project on a **GCP**, selects an existing **Google Kubernetes Cluster**. Deploys a cert manager and creates SSL certificate. Deploy **microservice application**.


## Dependices
| Dependence               | Description                                        |
|--------------------------|----------------------------------------------------|
|`static_global_IP_addres` | The global static ip address. It is necessary to change the line `kubernetes.io/ingress.global-static-ip-name: "ip-name"` in the file `microservices-demo/release/kubernetes-manifests.yaml` file.
|`DNS_name`                | A-record that must be created and tied to a previously created new IP address. It is necessary to change  the line `- host:"dns-name"` in the file `microservices-demo/release/kubernetes-manifests.yaml` file. It is also necessary to change the variables `dnsZones`, `dnsNames`, `project` and `email` in the `microservices-demo/cert.yaml` file for the necessary variables. More [here](https://cert-manager.io/docs/configuration/acme/dns01/google/). |
|`service_account`         | Service account for further work, the necessary roles will be described below.|

It is also necessary to change the path to the **Google Cloud Storage** in the `main.jenkins` file and change them accordingly  
in the manifest file. This path should be "hostname"/"project-id"/"image":"${GIT_COMMIT_MICRO} ". More [here](https://cloud.google.com/container-registry/docs/pushing-and-pulling).

## Requirements
1. google-cloud-sdk
`echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list`  
`sudo apt-get install apt-transport-https ca-certificates gnupg`  
`curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -`  
`sudo apt-get update && sudo apt-get install google-cloud-sdk`  
1. kubectl  
`sudo apt install kubectl -y`  
1. nodejs   
`curl -sL https://deb.nodesource.com/setup_10.x | bash`  
`sudo apt install nodejs -y`  
1. eslint   
`npm i -g eslint`  
1. go  
`curl -O https://storage.googleapis.com/golang/go1.12.9.linux-amd64.tar.gz`  
`tar -xvf go1.12.9.linux-amd64.tar.gz`  
`sudo chown -R jenkins:jenkins ./go`  
`sudo mv go /usr/local`  
1. python   
`sudo apt install python -y`  
1. pip      
`sudo apt install python-pip -y`  
1. flake8   
`pip install flake8`  
1. Java linter    
`wget https://github.com/checkstyle/checkstyle/releases/download/checkstyle-8.30/checkstyle-8.30-all.jar`   
`mv ./checkstyle-8.30-all.jar /`  
## IAM
In order to execute this module you must have a Service Account with the following roles:

Service account or user credentials with the following roles must be used to provision the resources of this module:

* Compute Viewer
* Kubernetes Engine Admin
* Kubernetes Engine Cluster Admin
* DNS Administrator
* Service Account User
* Storage Admin

## License
GNU General Public License v3.0

## Author Information
https://www.sxvova.opensource-ukraine.org/
