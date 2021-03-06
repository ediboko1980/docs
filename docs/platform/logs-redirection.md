# Introduction

By default, D2C stores [logs](/platform/logs) of containers itself. If you need to redirect your logs to any other storages, you can use this feature. It's available only on [business accounts](/accounts/plan).

## How to redirect logs of a container outside D2C

1. Open a [service page](/services/services/)
2. Find **Containers** block in the bottom of a page
3. Click **Log redirection**
4. Choose a [log provider](/providers/log-providers/)
5. Click confirm

!!! note

    All new logs of the service containers will be send to the selected log provider.
    You won't be able to see them in the D2C interface.

!!! note

    Logstash container should be online when you redirect logs of a service.


![Log redirection button](../img/new_interface/log_redirection_button.png)

![Log redirection window](../img/new_interface/log_redirection_window.png)

## Stacks for logs redirection

You can find a ready to use [Elastic](https://d2c.io/stackhub/elastic-stack) and [Logstash-InfluxDB-Grafana](https://d2c.io/stackhub/logstash-influxdb-grafana-stack) stacks in our Stackhub.

After you deploy a stack you need to add **Index pattern** for Kibana and **Data source** for Grafana. Read more in [Kibana](https://www.elastic.co/guide/en/kibana/current/tutorial-define-index.html) and [Grana](https://grafana.com/docs/grafana/latest/features/datasources/influxdb/) documentation.
