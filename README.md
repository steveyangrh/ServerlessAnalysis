## Getting started

### Platform specificions

Platform Choice: [AWS EKS](https://aws.amazon.com/eks/) (Preliminary) <br />
Docker Orchestration: [Kubernetes](https://kubernetes.io/) (Integrated with AWS EKS, no need to install or configure manually) <br />
Serverless Framework: Not yet selected ([OpenFaaS](https://www.openfaas.com/) (Recommended as it has better documentation and more stackoverflow stars), [Kubeless](https://kubeless.io/)) <br />
Programming Language Choice: Not yet selected (Jimmy Lin's paper used Python as Python packages have smaller sizes than Java packages) <br />


### Preperation

##### Step 1: Install the latest version of AWS CLI
```
pip install awscli --upgrade --user
```

##### Step 2: Configure the AWS CLI credentials using [AWS Credentials](https://docs.google.com/document/d/1YR27oAiMSkNcl4CCAiaQ6h-hg12sXD5WRzE_1wMKVVQ/edit?usp=sharing)

Type ```aws configure``` and enter your credentials

```
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: us-east-1
Default output format [None]: json
```

##### Step 3: Install eksctl <br />
(Note: Please use the following commands for installing eksctl, as they also automatically install ```kubectl``` and 
```aws-iam-authenticator``` for you, otherwise you may encounter verfication issues)


###### Step 3.1: If you don't have homebrew, install it with the following command
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

###### Step 3.2: Install the Weaveworks Homebrew tap
```
brew tap weaveworks/tap
```

###### Step 3.3: Install or upgrade eksctl
```
brew install weaveworks/tap/eksctl
```
if eksctl is already installed, run the following command to upgrade:
```
brew upgrade eksctl && brew link --overwrite eksctl
```

###### Step 3.4: Check if eksctl is successfully installed
```
eksctl version
```


### Setting up Kubernetes cluster (Only need to be done once)
Example from [examples](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)
```
eksctl create cluster \
--name test \
--version 1.14 \
--region us-east-1 \
--nodegroup-name standard-workers \
--node-type t3.medium \
--nodes 3 \
--nodes-min 1 \
--nodes-max 4 \
--node-ami auto
```
Use ```eksctl create cluster --help``` for more details.


After the cluster is created, use
```
kubectl get svc
```
to check available Kubernetes clusters.


If you encounter error ```"aws-iam-authenticator": executable file not found in $PATH```, then your AWS account credentials were not configured correctly.

##### Step 4: Specifying the alternative kubeconfig file for kubectl
```
export KUBECONFIG=~/.kube/eksctl/clusters/openfaas-eks
```



### Shutting down EKS cluster
Looks like we cannot shut down a cluster, we have to delete it ..... kinda stupid, later on we just gotta keep it running, it may cost come money, don't worry about it


### Deploying OpenFaaS on AWS EKS (Only need to be done once)

There is a [full tutorial](https://aws.amazon.com/blogs/opensource/deploy-openfaas-aws-eks/) on deploying OpenFaaS on AWS EKS, below shows the crutial steps.

If you have followed the steps above, you already have AWS ClI, eksctl CLI and Kubernetes CLI installed. In the next steps, we'll just install 

##### Step 1: Install Helm
First, create a Kubernetes service account for the server component of Helm called Tiller:
```
kubectl -n kube-system create sa tiller
kubectl create clusterrolebinding tiller-cluster-rule \
    --clusterrole=cluster-admin \
    --serviceaccount=kube-system:tiller
```
Now deploy Tiller into the cluster:
```
helm init --upgrade --service-account tiller
```

##### Step 2: Creating Kubernetes Namespaces for OpenFaaS
```
kubectl apply -f https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml
```

##### Step 3: Create OpenFaaS Credentials
Use [PASSWORD](https://docs.google.com/document/d/1YR27oAiMSkNcl4CCAiaQ6h-hg12sXD5WRzE_1wMKVVQ/edit?usp=sharing) as PASSWORD

```
kubectl -n openfaas create secret generic basic-auth \
--from-literal=basic-auth-user=admin \
--from-literal=basic-auth-password=$PASSWORD
```

##### Step 4: Install OpenFaaS with credentials enabled

###### Step 4.1: Add the OpenFaaS Helm chart repo 
```
helm repo add openfaas https://openfaas.github.io/faas-netes/
```

###### Step 4.2: Install OpenFaaS with authentication enabled by default using Helm
```
helm upgrade openfaas --install openfaas/openfaas \
    --namespace openfaas  \
    --set functionNamespace=openfaas-fn \
    --set serviceType=LoadBalancer \
    --set basic_auth=true \
    --set operator.create=true \
    --set gateway.replicas=2 \
    --set queueWorker.replicas=2
```

The settings above mean that functions will be installed into a separate namespace for easier management, a LoadBalancer will be created (read below), basic authentication will be used to protect the gateway UI, and then we are opting to use the OpenFaaS Operator to manage the cluster with operator.create=true. We set two replicas of the gateway to be available so that there is high availability, and two queue-workers for high availability and increased concurrent processing.

Run ```kubectl --namespace=openfaas get deployments -l "release=openfaas, app=openfaas"``` to verify your deployment, if you get at least ```1```, your deployment is correct.

###### Step 4.3: After LoadBalancer is created, then you can display the public IP address

Use ```kubectl get svc -n openfaas -o wide``` to display the public IP address

###### Step 4.4: Save the EXTERNAL-IP as an environment variable

Type ```export OPENFAAS_URL=$(kubectl get svc -n openfaas gateway-external -o  jsonpath='{.status.loadBalancer.ingress[*].hostname}'):8080 \ && echo Your gateway URL is: $OPENFAAS_URL```

###### Step 4.5: Save your login credentials to ``` ~/.openfaas/config.yaml```

Type ```echo $PASSWORD | faas-cli login --username admin --password-stdin```



### Deploying functions on OpenFaaS


### Useful Links

1. [Deploy the Kubernetes Web UI (Dashboard)](https://docs.aws.amazon.com/eks/latest/userguide/dashboard-tutorial.html)
2. [Understanding Kubernetes](https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes)

### Report Documentation Link
[Overleaf](https://www.overleaf.com/project/5dbb44b7d697d800012661ca)
