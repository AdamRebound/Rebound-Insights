# Rebound-Insights---Logstash
A repository for the logstash configuration work for the Insights project

Before launching LogStash pipeline, do next command:
```
gcloud container clusters get-credentials high-mem-cluster-1 --zone us-central1-a --project virtual-firefly-247112 \
&& kubectl port-forward --namespace kube-logging $(kubectl get pod --namespace kube-logging --selector="app=elasticsearch" --output jsonpath='{.items[0].metadata.name}') 8080:9200
```
It will open the connection to GCloud Kubernetise instance with ElasticSearch.


To start the LogStash and it's pipelines, do next:
```
docker-compose up
```