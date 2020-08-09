# Configuration

## Getting Started

Start by creating the following files:

### Environment variables

{% code title=".env" %}
```text
CLUSTER_NAME=elasticsearch-cluster
VERSION=7.8.1
```
{% endcode %}

### Basic configuration

{% code title="docker-compose.yml" %}
```yaml
version: "3.7"
x-environment:
  &base-env
  cluster.name: ${CLUSTER_NAME}
  discovery.seed_hosts: tiebreaker,node-1,node-2
  bootstrap.memory_lock: 'true'
  ES_JAVA_OPTS: '-Xms512m -Xmx512m'

services:
  node-1:
    image: docker.elastic.co/elasticsearch/elasticsearch:${VERSION}
    environment:
      <<: *base-env
      node.name: 'node-1'
    volumes:
      - es-data01:/usr/share/elasticsearch/data
    networks:
      - elastic
    ulimits:
      memlock:
        soft: -1
        hard: -1

  node-2:
    image: docker.elastic.co/elasticsearch/elasticsearch:${VERSION}
    environment:
      <<: *base-env
      node.name: 'node-2'
    volumes:
      - es-data02:/usr/share/elasticsearch/data
    networks:
      - elastic
    ulimits:
      memlock:
        soft: -1
        hard: -1

  tiebreaker:
    image: docker.elastic.co/elasticsearch/elasticsearch:${VERSION}
    environment:
      <<: *base-env
      node.voting_only: 'true'
      node.name: 'tiebreaker'
    ports:
      - "9200:9200"
    volumes:
      - es-data03:/usr/share/elasticsearch/data
    networks:
      - elastic
    ulimits:
      memlock:
        soft: -1
        hard: -1
  
networks:
  elastic:
    driver: bridge

volumes:
  es-data01:
    driver: local
  es-data02:
    driver: local
  es-data03:
    driver: local
```
{% endcode %}

### Bootstrap configuration

{% code title="docker-compose.bootstrap.yml" %}
```yaml
version: "3.7"
x-environments: &bootstrap-cluster
    cluster.initial_master_nodes: tiebreaker,node-1,node-2

services:
  tiebreaker:
    environment:
      <<: *bootstrap-cluster
  node-1:
    environment:
      <<: *bootstrap-cluster
  node-2:
    environment:
      <<: *bootstrap-cluster
```
{% endcode %}

