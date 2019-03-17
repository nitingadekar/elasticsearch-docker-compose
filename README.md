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










