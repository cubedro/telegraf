# Self Input Plugin

The `self` plugin collects metrics about the telegraf agent itself.

Note that some metrics are aggregates across all instances of one type of
plugin.

### Configuration:

```toml
# Collect statistics about itself
[[inputs.self]]
  ## If true, collect telegraf memory stats.
  # collect_memstats = true
```

### Measurements & Fields:

memstats are taken from the Go runtime: https://golang.org/pkg/runtime/#MemStats

- self\_memstats
    - alloc\_bytes
    - frees
    - heap\_alloc\_bytes
    - heap\_idle\_bytes
    - heap\_in\_use\_bytes
    - heap\_objects\_bytes
    - heap\_released\_bytes
    - heap\_sys\_bytes
    - mallocs
    - num\_gc
    - pointer\_lookups
    - sys\_bytes
    - total\_alloc\_bytes

agent stats collect aggregate stats on all telegraf plugins.

- self\_agent
    - gather\_errors
    - metrics\_dropped
    - metrics\_gathered
    - metrics\_written

self\_inputs\_\<plugin\_name\> stats collect aggregate stats on all input plugins
that are of the same input type.

- self\_inputs\_\<plugin\_name\>
    - gather\_time\_ns
    - metrics\_gathered

self\_outputs\_\<plugin\_name\> stats collect aggregate stats on all output plugins
that are of the same input type.

- self\_outputs\_\<plugin\_name\>
    - buffer\_limit
    - buffer\_size
    - metrics\_written
    - write\_time\_ns

self\_\<plugin\_name\> are metrics which are defined on a per-plugin basis, and
usually contain tags which differentiate each instance of a particular type of
plugin.

- self\_\<plugin\_name\>
    - individual plugin-specific fields, such as requests counts.

### Tags:

All measurements for specific plugins are tagged with information relevant
to each particular plugin.

### Example Output:

```
self_memstats,host=tars alloc_bytes=4325720i,frees=7460i,heap_alloc_bytes=4325720i,heap_idle_bytes=1433600i,heap_in_use_bytes=5283840i,heap_objects_bytes=9999i,heap_released_bytes=0i,heap_sys_bytes=6717440i,mallocs=17459i,num_gc=2i,pointer_lookups=7i,sys_bytes=11376888i,total_alloc_bytes=6748168i 1479295080000000000
self_agent,host=tars gather_errors=0i,metrics_dropped=0i,metrics_gathered=13i,metrics_written=12i 1479295080000000000
self_outputs_file,host=tars buffer_limit=10000i,buffer_size=0i,metrics_written=12i,write_time_ns=193407i 1479295080000000000
self_inputs_self,host=tars gather_time_ns=319155i,metrics_gathered=13i 1479295080000000000
self_inputs_http_listener,host=tars gather_time_ns=30677i,metrics_gathered=0i 1479295080000000000
self_http_listener,address=:8186,host=tars bytes_received=0i,not_founds_served=0i,pings_received=0i,pings_served=0i,queries_received=0i,queries_served=0i,requests_received=0i,requests_served=0i,writes_received=0i,writes_served=0i 1479295080000000000
```