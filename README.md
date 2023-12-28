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

## Install helm chart milvus with "values.yaml"
### Fill in kafka's username and password in the last line in "values.yaml"
```bash
helm upgrade --install milvus milvus/milvus -f values.yaml -n milvus --create-namespace
```

### Done!
