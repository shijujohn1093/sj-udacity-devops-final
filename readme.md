# Cloud DevOps Engineer Capstone Project

## Suumary

CI/CD pipeline for python flask applicaiton using rolling strategy. Implemented on cloud native design pattern using docker and kubernetes (EKS).


## Project files

1) application : Flask web applicaiton
    URL : http://a74144880f6c24870a0b7de18ff7218e-322643399.us-east-1.elb.amazonaws.com/about
2) k8s-cluster : Template to create kubernetes cluster
3) k8s-deployment : is for deployment services deployment which will triggered from jenkind


## About Project:

### Kubenetes Cluster

I created a CI/CD pipeline for a basic website that deploys to a cluster in AWS EKS. Find code at k8s-cluster/create-cluster.sh

### Applicaiton validation, build and deployment through CI/CD pipeline

* Print build and tag details to be created
* Replace build token in deployment and html file k8s-deployment/deployment.yaml, application/templates/about.html so that we can validate deployed version.
* Lint docker file
* Build docker image with tag
* Publish docker image to docker hub
* Deploy application through kubectl
* Deploy LoadBalancer service through kubectl

## Project Requirement

* Jenkins
* Blue Ocean Plugin in Jenkins
* Pipeline-AWS Plugin in Jenkins
* Docker
* AWS Cli
* Eksctl
* Kubectl


## Other References
http://eksworkshop.com/
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html#cliv2-linux-upgrade
https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html





