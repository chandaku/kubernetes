# Kubernetes Setup for Prometheus and Grafana

## Quick start

To quickly start all the things just do this:

```bash
cd kubernetes-prometheus
```
Modify namespace into ./manifests/prometheus/rbac.yaml as follows

```bash
subjects:
- kind: ServiceAccount
  name: prometheus-k8s
  namespace: default
```
  
execute 
```bash
./build.sh
```

It will generate manifests-all.yaml

--Run Following 

```bash
kubectl create -f manifests-all.yaml
```

This will setup necessary services and pods for Grafan/Prometheuse



## Import Dashboards

```bash
kubectl delete configmap grafana-import-dashboards
kubectl delete job grafana-import-dashboards

kubectl create configmap grafana-import-dashboards --from-file=prometheus-datasource.json=./manifests/grafana/datasource/prometheus-datasource.json --from-file=k8s-cluster-health-dashboard.json=./manifests/grafana/dashboard/k8s-cluster-health-dashboard.json --from-file=go-processes-for-kubernetes-dashboard.json=./manifests/grafana/dashboard/go-processes-for-kubernetes-dashboard.json --from-file=nodes-systems-stats-dashboard.json=./manifests/grafana/dashboard/nodes-systems-stats-dashboard.json --from-file=all-nodes-dashboard.json=./manifests/grafana/dashboard/all-nodes-dashboard.json --from-file=analysis-by-pod-dashboard.json=./manifests/grafana/dashboard/analysis-by-pod-dashboard.json


kubectl apply --filename ./manifests/grafana/import-dashboards/job.yaml
```


## More Dashboards

See grafana.net for some example [dashboards](https://grafana.net/dashboards) and [plugins](https://grafana.net/plugins).

- Configure [Prometheus](https://grafana.net/plugins/prometheus) data source for Grafana.<br/>
`Grafana UI / Data Sources / Add data source`
  - `Name`: `prometheus`
  - `Type`: `Prometheus`
  - `Url`: `http://prometheus:9090`
  - `Add`

- Import [Prometheus Stats](https://grafana.net/dashboards/2):<br/>
  `Grafana UI / Dashboards / Import`
  - `Grafana.net Dashboard`: `https://grafana.net/dashboards/2`
  - `Load`
  - `Prometheus`: `prometheus`
  - `Save & Open`

- Import [Kubernetes cluster monitoring](https://grafana.net/dashboards/162):<br/>
  `Grafana UI / Dashboards / Import`
  - `Grafana.net Dashboard`: `https://grafana.net/dashboards/162`
  - `Load`
  - `Prometheus`: `prometheus`
  - `Save & Open`
