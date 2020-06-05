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
kubectl get pods -l app=cassandra -o json | jq '.items[] | {"name": .metadata.name,"hostname": .spec.nodeName, "hostIP": .status.hostIP, "PodIP": .status.podIP}'
apt-get install build-essential
apt-get install make
wget http://python.org/ftp/python/2.7.6/Python-2.7.6.tgz
tar -xvzf Python-2.7.6.tgz
cd Python-2.7.6
./configure --prefix=/usr/local
make
make install

######Exit from the pod and execute following command to start cqlsh
kubectl exec -it cassandra-0 cqlsh







