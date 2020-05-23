## create ec2 key pair 
aws ec2 create-key-pair --key-name eks-cluster-prod

## create eks cluster with spot instances node group
eksctl create cluster -f eks-cluster-imagitics.yaml

## create eks cluster with spot instances node group
eksctl apply -f eks-cluster-imagitics.yaml

## deploy the autoscaler
kubectl apply -f autoscaler.yaml

###### put required annotation to the deployment:
kubectl -n kube-system annotate deployment.apps/cluster-autoscaler cluster-autoscaler.kubernetes.io/safe-to-evict="false"

## view cluster autoscaler logs
kubectl -n kube-system logs deployment.apps/cluster-autoscaler

## install helm - package manager of kubernetes
##### install latest version of Helm
cd 
mkdir helm && cd helm
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
cd -

##### add official stable Helm repository
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm version
helm repo list
helm repo update
helm search repo

## view your Cluster Autoscaler logs
kubectl -n kube-system logs -f deployment.apps/cluster-autoscaler

## deploy metrics server
DOWNLOAD_URL=$(curl --silent "https://api.github.com/repos/kubernetes-sigs/metrics-server/releases/latest" | jq -r .tarball_url) 
DOWNLOAD_VERSION=$(grep -o '[^/v]*$' <<< $DOWNLOAD_URL)
curl -Ls $DOWNLOAD_URL -o metrics-server-$DOWNLOAD_VERSION.tar.gz
mkdir metrics-server-$DOWNLOAD_VERSION
tar -xzf metrics-server-$DOWNLOAD_VERSION.tar.gz --directory metrics-server-$DOWNLOAD_VERSION --strip-components 1
kubectl apply -f metrics-server-$DOWNLOAD_VERSION/deploy/1.8+/

kubectl get deployment metrics-server -n kube-system

## deploy the K8s dashboard
export K8S_DASHBOARD=v2.0.0-beta4
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/$K8S_DASHBOARD/aio/deploy/recommended.yaml

##### create an admin user account
kubectl apply -f admin-service-account.yaml
   
##### access the dashboard
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-course-admin | awk '{print $1}')
kubectl proxy

##### open browser at 
`http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#!/login`

