# PostgreSQL_final
The task was to deploy a database on a PostgreSQL failover cluster.

*Scheme*

![Scheme](https://github.com/vadim-davydchenko/PostgreSQL_final/blob/master/task14_scheme.jpg)

Replication:
  - master (patroni)
  - replica:
    - Synchronous mode
    -  Async mode
    -  Standby mode
Condition:
- in case of a master crash -> first switch to a replica with synchronous replication (priority);
- if a dead instance of the previous master has not risen within 3 minutes, -> reserve input.
- patroni (cluster management) + etcd (cluster state storage)
- HAproxy on input, balance between two pgbouncers:
  - port 6432;
  - roundrobin balancer
- pgbouncer — two instance.
- Monitoring server - Prometheus. For it, information about system metrics (node_exporter) and information about the state of the cluster are collected from all machines (the master is polled via postgres_exporter)
- To display metrics - Grafana
