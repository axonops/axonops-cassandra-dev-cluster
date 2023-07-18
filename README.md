
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

By default the Cassandra ports are not exposed outside the Docker network. If you need to access Cassandra
from other applications this can be achieved by adding a `ports` section to one of the Cassandra node definitions,
for example:
```yaml
  cassandra-0:
    image: cassandra:4.0
    ports:
      - 9042:9042
...
```
