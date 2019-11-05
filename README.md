## Getting started

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
    ~ Using AWS EKS for managing Kubernetes cluster
    


### Deploying Kubeless on Kubernetes

### Deploying functions on Kubeless


### Report Documentation Link
[Overleaf](https://www.overleaf.com/project/5dbb44b7d697d800012661ca)
