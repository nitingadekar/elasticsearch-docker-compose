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
$ curl -XGET "http://13.232.14.66:9200/_cluster/health?pretty" # use pubic or private IP of your server for accessing
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


~ Thats it! you are good to go now.. ~
Thanks for visiting





