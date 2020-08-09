# Deploy a quick cluster

## Using docker-compose

This demonstrate how to use docker-compose to quickly deploy a cluster.

**Remember**: By default, a node is all of the following types: master-eligible, data, ingest, and \(if available\) machine learning. All data nodes are also transform nodes.

**PLEASE DO NOT RUN THIS CONFIGURATION IN PRODUCTION**

## Requirements

* Install [docker-compose](https://docs.docker.com/compose/install/)
* Set the `vm.max_map_count` kernel setting &gt;= _262144_

For more information about the kernel setting click [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#_set_vm_max_map_count_to_at_least_262144).

## Overview

Before working to work with the cluster, we must bootstrap it. Here is a short explanation of the steps you will do next:

1. Bootstrap the cluster
2. Verify the health of the cluster
3. Verify the status of the nodes
4. Restart the cluster

