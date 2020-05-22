## create ec2 key pair 
aws ec2 create-key-pair --key-name eks-cluster-prod

## create eks cluster with spot instances node group
eksctl apply -f eks-cluster-imagitics.yaml

## create eks cluster with spot instances node group
eksctl apply -f eks-cluster-imagitics.yaml

## deploy the autoscaler
kubectl apply -f autoscaler.yaml

###### put required annotation to the deployment:
kubectl -n kube-system annotate deployment.apps/cluster-autoscaler cluster-autoscaler.kubernetes.io/safe-to-evict="false"

## view cluster autoscaler logs
kubectl -n kube-system logs deployment.apps/cluster-autoscaler

## view your Cluster Autoscaler logs
kubectl -n kube-system logs -f deployment.apps/cluster-autoscaler