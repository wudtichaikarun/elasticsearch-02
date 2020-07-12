# Inspecting the cluster

- [add nodes to your cluster](https://www.elastic.co/guide/en/elasticsearch/reference/current/encrypting-communications-hosts.html)

At Kibana menu Dev Tools

```
// command
GET /_cluster/health

// output
{
  "cluster_name" : "elasticsearch",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 6,
  "active_shards" : 6,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}

```

```
// parameter v instructs Elasticsearch to include a descriptive header with in the output
// command
GET /_cat/nodes?v

// output
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
127.0.0.1           24         100  15    2.55                  dilmrt    *      wudtichais-MBP

```

```
// command
GET /_cat/indices?v


// output
health status index                          uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .kibana-event-log-7.8.0-000001 Ei_KIVn3Tk62LG-UYdDiZQ   1   0          2            0     10.4kb         10.4kb
green  open   .apm-custom-link               NWeq8tSJSTi-FZS8HwBhvg   1   0          0            0       208b           208b
green  open   .kibana_task_manager_1         4rpa9wJnSA2xtf_XkBLTJg   1   0          5            7       43kb           43kb
green  open   .apm-agent-configuration       omeRIQjuQFuDxQ5Tm7Suqg   1   0          0            0       208b           208b
green  open   .kibana_1
```

```
// command
GET /_cat/shards?v

// output
index                          shard prirep state   docs  store ip        node
.apm-custom-link               0     p      STARTED    0   208b 127.0.0.1 wudtichais-MBP
.kibana_1                      0     p      STARTED   19   76kb 127.0.0.1 wudtichais-MBP
.kibana-event-log-7.8.0-000001 0     p      STARTED    2 10.4kb 127.0.0.1 wudtichais-MBP
ilm-history-2-000001           0     p      STARTED             127.0.0.1 wudtichais-MBP
.kibana_task_manager_1         0     p      STARTED    5   43kb 127.0.0.1 wudtichais-MBP
.apm-agent-configuration       0     p      STARTED    0   208b 127.0.0.1 wudtichais-MBP

```
