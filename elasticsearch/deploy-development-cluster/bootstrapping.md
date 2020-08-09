# Bootstrapping

## Bootstrap the cluster

To begin with, start by bootstrapping the cluster:

```text
docker-compose -f docker-compose.yml -f docker-compose.bootstrap.yml up
```

### Health of the cluster

Verifying the cluster health is a good starting point to see if everything went well after bootstrapping the cluster:

```text
$ curl -s "127.0.0.1:9200/_cluster/health?pretty=true

{
  "cluster_name": "elasticsearch-cluster",
  "status": "green",
  "timed_out": false,
  "number_of_nodes": 3,
  "number_of_data_nodes": 3,
  "active_primary_shards": 0,
  "active_shards": 0,
  "relocating_shards": 0,
  "initializing_shards": 0,
  "unassigned_shards": 0,
  "delayed_unassigned_shards": 0,
  "number_of_pending_tasks": 0,
  "number_of_in_flight_fetch": 0,
  "task_max_waiting_in_queue_millis": 0,
  "active_shards_percent_as_number": 100
}
```

If you have `jq` installed, you can filter the important values:

```text
$ curl -s "127.0.0.1:9200/_cluster/health?pretty=true" | jq '{cluster_name: .cluster_name, nb_of_nodes: .number_of_nodes, status: .status}'

{
  "cluster_name": "elasticsearch-cluster",
  "nb_of_nodes": 3,
  "status": "green"
}
```

### Verifying the status of the nodes

```text
$ curl "127.0.0.1:9200/_cat/nodes?v&h=name,master,role,ip,port"

name       master role    ip         port
node-1     -      dilmrt  172.18.0.3 9300
node-2     *      dilmrt  172.18.0.4 9300
tiebreaker -      dilmrtv 172.18.0.2 9300
```

More information about ['`/_cat/nodes`'](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-nodes.html)

### About the "_role"_ column

| Node role | Value |
| :--- | :--- |
| Data | d |
| Ingest | i |
| Master-Eligible | m |
| Machine Learning | l |
| Voting-Only | v |
| Transform | t |
| Remote Cluster client | r |
| Coordinating | - |

