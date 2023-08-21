# AxonOps Cassandra Development Cluster

This Docker Compose file will create a Cassandra cluster containing 3 nodes along with AxonOps for
cluster monitoring and management.

## Getting Started

1. Download `docker-compose.yml` from this repository
2. Run `docker compose up -d` from the directory containing `docker-compose.yml`
3. Wait a few minutes for the containers to start up and initialise
4. Access the AxonOps web interface at http://127.0.0.1:3000/

### Accessing Cassandra

Once the cluster has started up you can run `cqlsh` inside one of the Cassandra containers by using this command:
```bash
docker compose exec -it cassandra-0 cqlsh
```

The CQL port for each node is also exposed on the loopback IP address as follows:
```
cassandra-0: 127.0.0.1:9042
cassandra-1: 127.0.0.1:9043
cassandra-2: 127.0.0.1:9044
```
Client applications running on the host should be able to use these addresses to connect to Cassandra.
