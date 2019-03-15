# elasticsearch-docker-compose
Elasticsearch docker compose for production 

For linux (Amazon linux 1, RHEL7.5, Centos7) set vm.max_map_count to 262144
Go to /etc/sysctl.conf and add below line
vm.max_map_count=262144

$ grep vm.max_map_count /etc/sysctl.conf
vm.max_map_count=262144

To apply the setting on a live system type: sysctl -w vm.max_map_count=262144


Install Docker with sudo previleges
$ sudo yum install docker* -y
$ service docker start
or 
$ systemctl start docker

Use the docker-compose.yml file in current repo and make modification if required.
Run the docker compose command to run the docker cluster.
$ docker-compose up 



