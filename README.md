## Getting started

### Platform specificions

Platform Choice: [AWS EKS](https://aws.amazon.com/eks/) (Preliminary) <br />
Docker Orchestration: [Kubernetes](https://kubernetes.io/) (Integrated with AWS EKS) <br />
Serverless Framework: Not yet selected ([OpenFaas](https://www.openfaas.com/), [Kubeless](https://kubeless.io/)) <br />
Programming Language Choice: Not yet selected (Jimmy Lin's paper used Python as Python packages have smaller sizes than Java packages) <br />


### Preperation

Step 1: Install the latest version of AWS CLI
```
pip install awscli --upgrade --user
```

Step 2: Configure the AWS CLI credentials using [AWS Credentials](https://docs.google.com/document/d/1YR27oAiMSkNcl4CCAiaQ6h-hg12sXD5WRzE_1wMKVVQ/edit?usp=sharing)

```
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: us-east-1
Default output format [None]: json
```

Step 3: Install eksctl
(Note: Please use the following command for installing eksctl, as they also automatically install ```kubectl``` and 
```aws-iam-authenticator``` for you.)


Step 3.1: If you don't have homebrew, install it with the following command
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Step 3.2: Install the Weaveworks Homebrew tap
```
brew tap weaveworks/tap
```

Step 3.3: Install or upgrade eksctl
```
brew install weaveworks/tap/eksctl
```
if eksctl is already installed, run the following command to upgrade:
```
brew upgrade eksctl && brew link --overwrite eksctl
```

Step 3.4: Check if eksctl is successfully installed
```
eksctl version
```


### Setting up Kubernetes cluster
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


### Deploying Kubeless on Kubernetes

### Deploying functions on Kubeless


### Report Documentation Link
[Overleaf](https://www.overleaf.com/project/5dbb44b7d697d800012661ca)
