**Configuration**

Setup the aws-cli to Amazon EKS using Access Key
```
aws configure
```

**Installation on Cluster with eksctl**

```
# Linux
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin

# macOS
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl

# Windows
chocolatey install eksctl
```

**Amazon EKS Cluster Deployment**
After installation, create the cluster deployment

```
kubectl create cluster --name mwa --version 1.20 --with-oidc --without-nodegroup
```

To check the deployment cluster node

```
kubectl get nodes

eksctl create nodegroup --cluster mwa --name mwa-node \
                        --node-type t2.micro --nodes 3 --nodes-min 1 --nodes-max 4 \
                        --ssh-access --ssh-public-key xxxxxx --managed
```

Check node status

```
kubectl get nodes

kubectl apply -f grafana-deployment.yaml \
              -f grafana-service.yaml \
              -f influx-deployment.yaml \
              -f influx-service.yaml \
              -f telegraf-deployment.yaml

kubectl expose deployment grafana --type LoadBalancer --name grafexp
```

To check for the services

```
kubectl get services grafexp -o yaml
```

Open the browser on port 3000

**Delete Cluster**

To delete the cluster

```
eksctl --cluster mwa delete nodegroup --name mwa-nodes
eksctl delete cluster --name mwa
```
