# Cassandra with AxonOps in Kubernetes

The following configuration can be used to create an Apache Cassandra cluster that uses
[AxonOps](https://axonops.com) for monitoring. Whilst the [docker-compose](../docker-comopse.yml) in the 
parent directory is meant to run a local AxonOps server with all its dependencies,
this Kubernetes condfiguration uses the AxonOps SaaS platform.

## AxonOps Agent


The main configuration for AxonOps is the [axonops.yaml](./resources/axonops.yaml) config file.

```yaml
    axon-server:
        hosts: "axon-server-svc"
        port: 1888
    axon-agent:
        org: demo
        tls:
          mode: "disabled"
    disable_command_exec: false
    kubeconfig:
      cadvisor:
        enabled: false
        namespace: kube-system
```

The defaults are probably good for most people.

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
kubectl apply -f resources/
```
