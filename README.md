# elasticsearch-docker-compose

## Elasticsearch docker compose for production 

### Prequites
For linux (Amazon linux 1, RHEL7.5, Centos7) set `vm.max_map_count` to `262144`

Go to /etc/sysctl.conf and add below line

`vm.max_map_count=262144`

``` bash
$ grep vm.max_map_count /etc/sysctl.conf

vm.max_map_count=262144
```

To apply the setting on a live system type: sysctl -w vm.max_map_count=262144


### Install Docker with sudo previleges

``` bash

$ sudo yum install docker* -y

$ service docker start
```
or 

```bash 
$ systemctl start docker
```

Use the  "docker-compose.yml" file in current repo and make modification if required.

Run the docker compose command to run the docker cluster.
```
$ docker-compose up 
```

#### Possible Issue: docker-compose command not found: Refer https://github.com/docker/compose/releases
``` bash
$ docker-cpmpose
-bash: docker-cpmpose: command not found
```
Install docker compose by installing below binaries:

Download binaries from github for docker-compose
``` bash 
$ curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```
Give executable permissions to the binary
```
$ chmod +x /usr/local/bin/docker-compose
```
Create symbolic link to the executable in binaries
```
$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
Now run below command right away in the directory with docker-compose.yml

` $ docker-compose up `


## Using the elasticsearch service

Once the compose file is running, check the container status by 
``` bash
$ docker ps 
```
To check if the running continer ports are exposed and mapped, check all active ports and services running 
``` bash
$ netstat -tunlp
```
Start accessing the elasticsearch service on port 9200 port and localhost or from remote servers using the port 9200 and IP address

```
$ telnet localhost 9200
```

``` bash 
$ curl -XGET "http://localhost:9200/_cluster/health?pretty" # use pubic or private IP of your server for accessing
{
  "cluster_name" : "elasticsearch",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 0,
  "active_shards" : 0,
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

#### How to list the stats of index
`$ curl -X GET "localhost:9200/index1,index2/_stats"
`

```
`{"_shards":{"total":10,"successful":10,"failed":0},"_all":{"primaries":{"docs":{"count":2563,"deleted":1},"store":{"size_in_bytes":571483},"indexing":{"index_total":75324,"index_time_in_millis":14716,"index_current":0,"index_failed":0,"delete_total":6531,"delete_time_in_millis":257,"delete_current":0,"noop_update_total":0,"is_throttled":false,"throttle_time_in_millis":0},"get":{"total":0,"time_in_millis":0,"exists_total":0,"exists_time_in_millis":0,"missing_total":0,"missing_time_in_millis":0,"current":0},"search":{"open_contexts":0,"query_total":21263,"query_time_in_millis":5209,"query_current":0,"fetch_total":3845,"fetch_time_in_millis":1304,"fetch_current":0,"scroll_total":111,"scroll_time_in_millis":7470709,"scroll_current":0,"suggest_total":0,"suggest_time_in_millis":0,"suggest_current":0},"merges":{"current":0,"current_docs":0,"current_size_in_bytes":0,"total":0,"total_time_in_millis":0,"total_docs":0,"total_size_in_bytes":0,"total_stopped_time_in_millis":0,"total_throttled_time_in_millis":0,"total_auto_throttle_in_bytes":104857600},"refresh":{"total":327,"total_time_in_millis":6832,"listeners":0},"flush":{"total":50,"periodic":0,"total_time_in_millis":598},"warmer":{"current":0,"total":222,"total_time_in_millis":28},"query_cache":{"memory_size_in_bytes":0,"total_count":0,"hit_count":0,"miss_count":0,"cache_size":0,"cache_count":0,"evictions":0},"fielddata":{"memory_size_in_bytes":0,"evictions":0},"completion":{"size_in_bytes":0},"segments":{"count":5,"memory_in_bytes":25980,"terms_memory_in_bytes":21467,"stored_fields_memory_in_bytes":1568,"term_vectors_memory_in_bytes":0,"norms_memory_in_bytes":2560,"points_memory_in_bytes":5,"doc_values_memory_in_bytes":380,"index_writer_memory_in_bytes":0,"version_map_memory_in_bytes":0,"fixed_bit_set_memory_in_bytes":0,"max_unsafe_auto_id_timestamp":-1,"file_sizes":{}},"translog":{"operations":9095,"size_in_bytes":1059187,"uncommitted_operations":0,"uncommitted_size_in_bytes":275,"earliest_last_modified_age":0},"request_cache":{"memory_size_in_bytes":0,"evictions":0,"hit_count":0,"miss_count":0},"recovery":{"current_as_source":0,"current_as_target":0,"throttle_time_in_millis":0}},"total":{"docs":{"count":5126,"deleted":2},"store":{"size_in_bytes":1142966},"indexing":{"index_total":150648,"index_time_in_millis":25203,"index_current":0,"index_failed":0,"delete_total":13062,"delete_time_in_millis":546,"delete_current":0,"noop_update_total":0,"is_throttled":false,"throttle_time_in_millis":0},"get":{"total":0,"time_in_millis":0,"exists_total":0,"exists_time_in_millis":0,"missing_total":0,"missing_time_in_millis":0,"current":0},"search":{"open_contexts":0,"query_total":40730,"query_time_in_millis":10113,"query_current":0,"fetch_total":7368,"fetch_time_in_millis":2508,"fetch_current":0,"scroll_total":225,"scroll_time_in_millis":15178307,"scroll_current":0,"suggest_total":0,"suggest_time_in_millis":0,"suggest_current":0},"merges":{"current":0,"current_docs":0,"current_size_in_bytes":0,"total":0,"total_time_in_millis":0,"total_docs":0,"total_size_in_bytes":0,"total_stopped_time_in_millis":0,"total_throttled_time_in_millis":0,"total_auto_throttle_in_bytes":209715200},"refresh":{"total":653,"total_time_in_millis":11249,"listeners":0},"flush":{"total":100,"periodic":0,"total_time_in_millis":1181},"warmer":{"current":0,"total":442,"total_time_in_millis":39},"query_cache":{"memory_size_in_bytes":0,"total_count":0,"hit_count":0,"miss_count":0,"cache_size":0,"cache_count":0,"evictions":0},"fielddata":{"memory_size_in_bytes":0,"evictions":0},"completion":{"size_in_bytes":0},"segments":{"count":10,"memory_in_bytes":51960,"terms_memory_in_bytes":42934,"stored_fields_memory_in_bytes":3136,"term_vectors_memory_in_bytes":0,"norms_memory_in_bytes":5120,"points_memory_in_bytes":10,"doc_values_memory_in_bytes":760,"index_writer_memory_in_bytes":0,"version_map_memory_in_bytes":0,"fixed_bit_set_memory_in_bytes":0,"max_unsafe_auto_id_timestamp":-1,"file_sizes":{}},"translog":{"operations":18190,"size_in_bytes":2118374,"uncommitted_operations":0,"uncommitted_size_in_bytes":550,"earliest_last_modified_age":0},"request_cache":{"memory_size_in_bytes":0,"evictions":0,"hit_count":0,"miss_count":0},"recovery":{"current_as_source":0,"current_as_target":0,"throttle_time_in_millis":0}}},"indices":{"nse":{"primaries":{"docs":{"count":2563,"deleted":1},"store":{"size_in_bytes":571483},"indexing":{"index_total":75324,"index_time_in_millis":14716,"index_current":0,"index_failed":0,"delete_total":6531,"delete_time_in_millis":257,"delete_current":0,"noop_update_total":0,"is_throttled":false,"throttle_time_in_millis":0},"get":{"total":0,"time_in_millis":0,"exists_total":0,"exists_time_in_millis":0,"missing_total":0,"missing_time_in_millis":0,"current":0},"search":{"open_contexts":0,"query_total":21263,"query_time_in_millis":5209,"query_current":0,"fetch_total":3845,"fetch_time_in_millis":1304,"fetch_current":0,"scroll_total":111,"scroll_time_in_millis":7470709,"scroll_current":0,"suggest_total":0,"suggest_time_in_millis":0,"suggest_current":0},"merges":{"current":0,"current_docs":0,"current_size_in_bytes":0,"total":0,"total_time_in_millis":0,"total_docs":0,"total_size_in_bytes":0,"total_stopped_time_in_millis":0,"total_throttled_time_in_millis":0,"total_auto_throttle_in_bytes":104857600},"refresh":{"total":327,"total_time_in_millis":6832,"listeners":0},"flush":{"total":50,"periodic":0,"total_time_in_millis":598},"warmer":{"current":0,"total":222,"total_time_in_millis":28},"query_cache":{"memory_size_in_bytes":0,"total_count":0,"hit_count":0,"miss_count":0,"cache_size":0,"cache_count":0,"evictions":0},"fielddata":{"memory_size_in_bytes":0,"evictions":0},"completion":{"size_in_bytes":0},"segments":{"count":5,"memory_in_bytes":25980,"terms_memory_in_bytes":21467,"stored_fields_memory_in_bytes":1568,"term_vectors_memory_in_bytes":0,"norms_memory_in_bytes":2560,"points_memory_in_bytes":5,"doc_values_memory_in_bytes":380,"index_writer_memory_in_bytes":0,"version_map_memory_in_bytes":0,"fixed_bit_set_memory_in_bytes":0,"max_unsafe_auto_id_timestamp":-1,"file_sizes":{}},"translog":{"operations":9095,"size_in_bytes":1059187,"uncommitted_operations":0,"uncommitted_size_in_bytes":275,"earliest_last_modified_age":0},"request_cache":{"memory_size_in_bytes":0,"evictions":0,"hit_count":0,"miss_count":0},"recovery":{"current_as_source":0,"current_as_target":0,"throttle_time_in_millis":0}},"total":{"docs":{"count":5126,"deleted":2},"store":{"size_in_bytes":1142966},"indexing":{"index_total":150648,"index_time_in_millis":25203,"index_current":0,"index_failed":0,"delete_total":13062,"delete_time_in_millis":546,"delete_current":0,"noop_update_total":0,"is_throttled":false,"throttle_time_in_millis":0},"get":{"total":0,"time_in_millis":0,"exists_total":0,"exists_time_in_millis":0,"missing_total":0,"missing_time_in_millis":0,"current":0},"search":{"open_contexts":0,"query_total":40730,"query_time_in_millis":10113,"query_current":0,"fetch_total":7368,"fetch_time_in_millis":2508,"fetch_current":0,"scroll_total":225,"scroll_time_in_millis":15178307,"scroll_current":0,"suggest_total":0,"suggest_time_in_millis":0,"suggest_current":0},"merges":{"current":0,"current_docs":0,"current_size_in_bytes":0,"total":0,"total_time_in_millis":0,"total_docs":0,"total_size_in_bytes":0,"total_stopped_time_in_millis":0,"total_throttled_time_in_millis":0,"total_auto_throttle_in_bytes":209715200},"refresh":{"total":653,"total_time_in_millis":11249,"listeners":0},"flush":{"total":100,"periodic":0,"total_time_in_millis":1181},"warmer":{"current":0,"total":442,"total_time_in_millis":39},"query_cache":{"memory_size_in_bytes":0,"total_count":0,"hit_count":0,"miss_count":0,"cache_size":0,"cache_count":0,"evictions":0},"fielddata":{"memory_size_in_bytes":0,"evictions":0},"completion":{"size_in_bytes":0},"segments":{"count":10,"memory_in_bytes":51960,"terms_memory_in_bytes":42934,"stored_fields_memory_in_bytes":3136,"term_vectors_memory_in_bytes":0,"norms_memory_in_bytes":5120,"points_memory_in_bytes":10,"doc_values_memory_in_bytes":760,"index_writer_memory_in_bytes":0,"version_map_memory_in_bytes":0,"fixed_bit_set_memory_in_bytes":0,"max_unsafe_auto_id_timestamp":-1,"file_sizes":{}},"translog":{"operations":18190,"size_in_bytes":2118374,"uncommitted_operations":0,"uncommitted_size_in_bytes":550,"earliest_last_modified_age":0},"request_cache":{"memory_size_in_bytes":0,"evictions":0,"hit_count":0,"miss_count":0},"recovery":{"current_as_source":0,"current_as_target":0,"throttle_time_in_millis":0}}}}}``
```
Note: Copy the json output and use online tools to convert the json to csv and get table format output

~ Thats it! you are good to go now.. ~
Thanks for reading.





