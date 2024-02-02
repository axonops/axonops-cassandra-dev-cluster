# Cassandra with AxonOps in Docker

## AxonOps Agent

Please log in to https://console.axonops.cloud and follow the intructions to get
and organization and an agent key.

Then, edit the `axonops.yaml` config to look like:

```yaml
    axon-server:
        hosts: "agents.axonops.cloud"
    axon-agent:
        key: your-key
        org: your-org
    disable_command_exec: false
    kubeconfig:
      cadvisor:
        enabled: false
        namespace: kube-system
```

If you use `cadvisor` in your cluster, you'll be able to get additional disk metrics.
Select `enabled: true` in this case and amend the namespace where cadvisor is installed if required

## Configuration

### Namespace

This template uses the `axonops` namespace. If you prefer to use another name, you'll need to update the following:

- All entries or `namepace: axonops`
- The cassandra seeds value

```
"cassandra-0.cassandra.axonops.svc.cluster.local" => "cassandra-0.cassandra.NAMESPACE.svc.cluster.local"
```

### Limits

You may also need to update the limits for your set up 

```yaml
          resources:
            requests:
              cpu: "500m"
              memory: 500Mi
            limits:
              cpu: "1"
              memory: 1024Mi
```

### Replicas

The default is to start up three cassandra nodes. Adjust the [replica count](cassandra.yaml) if you would like more.

## k3s

If you do not have a Kubernetes cluster to use, you can for example use `k3s`. The [k3s](./k3s) folder contains a docker-compose to start it up:

```sh
cd k3s
export K3S_TOKEN=${RANDOM}${RANDOM}${RANDOM}
export KUBECONFIG=$(pwd)/kubeconfig.yaml

docker-compose up
```

## Install

```sh
kubectl create ns axonops
kubectl apply -f axonops-sa.yaml
kubectl apply -f axonops.yaml
kubectl apply -f cassandra.yaml
```
