kubectl create -f gp2-storage-class.yaml 
kubectl apply -f pvcs.yaml 
kubectl apply -f cassandra.yaml
kubectl apply -f cassandra-service.yaml  
kubectl get svc cassandra

######Connect to a pod and figure out the fqdn of cassandra using nslookup
kubectl describe pods cassandra-0
kubectl get statefulset cassandra 
kubectl exec -it cassandra-0 -- nodetool status
kubectl exec -it cassandra-0 /bin/bash
apt-get update
apt-get install dnsutils
nslookup cassandra



