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
Instructions to work with logstash deploy and tasks inside kubernetes:
1. kubectl -n kube-logging delete secret generic logstash-config - deletes old logstash
2. kubectl -n kube-logging delete secret generic logstash-pipelines - delete old pipeline
3. kubectl -n kube-logging create secret generic logstash-config --from-file=./logstash.yml - create new logstash
4. kubectl -n kube-logging create secret generic logstash-pipelines --from-file=./orders.logstash.conf --from-file=./returns.logstash.conf --from-file=./trackings.logstash.conf - create new pipelines (they can be changed in appropriate conf files)
5. kubectl cp lookups/* kube-logging/$(kubectl -n kube-logging get pod | grep logstash | grep Running | awk '{print $1}'):/usr/share/logstash/lookups/ - upload new lookups if necessary
6. kubectl -n kube-logging scale deployment logstash-deployment --replicas=0 && kubectl -n kube-logging scale deployment logstash-deployment --replicas=1 - restart logstash deploy
