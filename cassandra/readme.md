kubectl create -f gp2-storage-class.yaml --namespace=imagitics-cluster
kubectl apply -f pvcs.yaml --namespace=imagitics-cluster
kubectl apply -f cassandra.yaml --namespace=imagitics-cluster 


