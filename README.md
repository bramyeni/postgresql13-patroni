# PostgreSQL 13 Read Replica using Patroni

Deploy PostgreSQL 13 Master + multiple Read Replicas on a docker container

## Assumptions

Master     : 192.168.0.158
Replica #1 : 192.168.0.178
Replica #2 : 192.168.0.188
Persistant disk for pgsql DB : /opt/pgsql13
etcd       : Running on master (you can duplicate etcd on multiple nodes or runs separately)

## Dockerfile
i have created a dockerfile with based image alpine edge

## How to Build the image
Get a dockerfile from this repo, let say you download it into /opt

<pre>
cd /opt
docker build --tag postgresql13 -f ./Dockerfile-pgsql13-alpine /mnt
</pre>

## Deploy PostgreSQL Master/Primary
<pre>
docker run -it --net host --name pgsql13 -e PRIMARYDB=yes -e RUNETCD=yes -e INITETCD=yes -e INITDB=yes -v /opt/pgsql13:/var/lib/postgresql postgresql13
</pre>

## Deploy PostgreSQL as Replica #1
<pre>
docker run -it --net host --name pgsql13repl -e MEMBERNAME=member_1 -e INITDB=yes -e ETCDIPPORT1="192.168.0.158:2379" -v /opt/pgsql13:/var/lib/postgresql postgresql13
</pre>

## Deploy PostgreSQL as Replica #2 
<pre>
docker run -it --net host --name pgsql13repl -e MEMBERNAME=member_2 -e INITDB=yes -e ETCDIPPORT1="192.168.0.158:2379" -v /opt/pgsql13:/var/lib/postgresql postgresql13
</pre>

## Deploy PostgreSQL as Replica #n
<pre>
docker run -it --net host --name pgsql13repl -e MEMBERNAME=member_n -e INITDB=yes -e ETCDIPPORT1="192.168.0.158:2379" -v /opt/pgsql13:/var/lib/postgresql postgresql13
</pre>


## Combine with openvpn 
You can also combine this to use openvpn so postgresql can have replication between on-premise and on-cloud  
