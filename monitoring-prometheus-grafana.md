# Installing Prometheus & Grafana

## Prometheus
kubectl create namespace prometheus
helm install prometheus stable/prometheus \
--namespace prometheus \
--set alertmanager.persistentVolume.storageClass="io1" \
--set server.persistentVolume.storageClass="io1"

##### set port forwarding to access Prometheus UI:
kubectl port-forward svc/prometheus-server 8888:80

##### access Prometheus UI from your *local* browser:
  * http://localhost:8888

## Grafana

kubectl create namespace grafana
helm install grafana stable/grafana \
--namespace grafana \
--set persistence.storageClassName="io1" \
--set adminPassword='GrafanaAdm!n' \
--set datasources."datasources\.yaml".apiVersion=1 \
--set datasources."datasources\.yaml".datasources[0].name=Prometheus \
--set datasources."datasources\.yaml".datasources[0].type=prometheus \
--set datasources."datasources\.yaml".datasources[0].url=http://prometheus-server.prometheus.svc.cluster.local \
--set datasources."datasources\.yaml".datasources[0].access=proxy \
--set datasources."datasources\.yaml".datasources[0].isDefault=true \
--set service.type=LoadBalancer

##### get admin password for logging in
kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

##### open Grafana
kubectl get svc - grafana grafana```

##### import dashboard(s)
* navigate to https://grafana.com/grafana/dashboards and select preconfigured dashboard for monitoring K8s cluster