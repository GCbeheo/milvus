# Install milvus with external etcd and kafka

## helm repo add
```bash
helm repo add milvus https://zilliztech.github.io/milvus-helm/
helm repo update
```

## Install helm chart kafka with "values_kafka.yaml"
```bash
helm upgrade --install kafka oci://registry-1.docker.io/bitnamicharts/kafka -f values_kafka.yaml -n kafka --create-namespace
```

## Install helm chart etcd with "values_etcd.yaml"
```bash
helm upgrade --install etcd oci://registry-1.docker.io/bitnamicharts/etcd -f values_etcd.yaml -n etcd --create-namespace
```

## Install helm chart pulsar with "values_pulsar.yaml"
```bash
kubectl create ns cert-manager
git clone https://github.com/apache/pulsar-helm-chart
cd pulsar-helm-chart
chmod +x scripts/cert-manager/install-cert-manager.sh
./scripts/cert-manager/install-cert-manager.sh
kubectl create ns pulsar
chmod +x scripts/pulsar/prepare_helm_release.sh
./scripts/pulsar/prepare_helm_release.sh -n pulsar -k pulsar
cd ..
rm -rf pulsar-helm-chart
helm repo add apache https://pulsar.apache.org/charts
helm repo update
helm install pulsar apache/pulsar --timeout 10m -f values_pulsar.yaml -n pulsar
```

## Install helm chart milvus with "values.yaml"
### Fill externalKafka with value: "username", "password" in "values.yaml"
### Fill externalPulsar with value: "host", "authPlugin", "authParams" in "values.yaml"
### You can edit pvc storage
```bash
helm upgrade --install milvus milvus/milvus -f values.yaml -n milvus --create-namespace
```

### Done!
