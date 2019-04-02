# etcd playground
![etcd Logo](https://github.com/etcd-io/etcd/blob/master/logos/etcd-horizontal-color.svg)

 A safe place to experiment with _[etcd](https://etcd.io)_ distributed key value store.

# Description
 
  This is a safe workspace and playground open to anyone looking to experiment with _[etcd](https://etcd.io)_ distributed key value store. The environment targets _dev/test_ usage and is not adviced to be used in production.

# Dependencies

 The workspace depneds on:
 - [etcd](https://etcd.io)
 - [direnv](https://direnv.net/)
 - [goreman](https://github.com/mattn/goreman)
 - [curl](https://curl.haxx.se/)
 
# What is etcd ?
 "etcd is an open-source distributed key value store that provides shared configuration and service discovery for Container Linux clusters. etcd runs on each machine in a cluster and gracefully handles leader election during network partitions and the loss of the current leader"[ref](https://coreos.com/etcd/docs/latest/getting-started-with-etcd.html).

  
# How to 

## Initialise the workspace
```
$ direnv allow 
```

Initialization will create the '${PWD}/bin/' folder and will download any required binaries e.g etcd, goreman etc.

Below there are some very basic tasks you may need to test against.

##  Start a single etcd server 

```
$ etcd
2018-12-22 21:37:52.042182 I | etcdmain: etcd Version: ...
```

##  Start an etcd cluster

```
$ goreman start
goreman start                                                                                                                                   git:(master↓5↑2|✚2…
21:28:50 etcd2 | Starting etcd2 on port 5001                                                                                                                                                         
21:28:50 proxy | Starting proxy on port 5003                                                                                                                                                         
21:28:50 etcd1 | Starting etcd1 on port 5000                                                                                                                                                         
21:28:50 etcd3 | Starting etcd3 on port 5002 
...                                                                                        
```

_NOTE: by default a three node cluster is spawn please advice procfile_

## Verify the server/cluster status

```
$ curl -L http://127.0.0.1:2379/health 
member 8211f1d0f64f3269 is healthy: got healthy result from http://127.0.0.1:3379
member 91bc3c398fb3c146 is healthy: got healthy result from http://127.0.0.1:4379
member fd422379fda50e48 is healthy: got healthy result from http://127.0.0.1:5379
```

## Enumerate all cluster members
```
$ etcdctl member list 
8211f1d0f64f3269: name=infra1 peerURLs=http://127.0.0.1:12380 clientURLs=http://127.0.0.1:3379 isLeader=false
91bc3c398fb3c146: name=infra2 peerURLs=http://127.0.0.1:22380 clientURLs=http://127.0.0.1:4379 isLeader=false
fd422379fda50e48: name=infra3 peerURLs=http://127.0.0.1:32380 clientURLs=http://127.0.0.1:5379 isLeader=true
```

## Verify clusters health
```
$ etcdctl cluster-health                                                                                                                           
```

## Set the value of an example key 
```
$ etcdctl set /key1 "value1"
value1
```

## Retrieve example key value
```
$ etcdctl get /key1
value1
```

## Delete example key
```
etcdctl rm /key1
```

## Set the value of an example key using the HTTP-based API
```
$ curl -L -X PUT http://127.0.0.1:2379/v2/keys/key2 -d value="value2"
```

## Retrive example key vaue using the HTTP-based API
```
$ curl -L http://127.0.0.1:2379/v2/keys/key2
{"action":"get","node":{"key":"/key2","value":"value2","modifiedIndex":19,"createdIndex":19}}
```
## Create a directory
```
$ etcdctl mkdir /tdir 
```

## Verify dir creation
```
$ etcdctl ls
/tdir

```
## Create a timed key with 30 sec time-to-live
```
$ etcdctl mk /timedkey "It's going to expire in 30 sec" --ttl 30
```

Check afer a couple seconds and after 30 seconds using the command below:

```
$ etcdctl get /timedkey "It's going to expire in 30 sec" --ttl 30
```

# References and links
 - [etcd](https://etcd.io)
 - [etcd documentation (latest)](https://coreos.com/etcd/docs/latest/)
 - [Getting started with etcd](https://coreos.com/etcd/docs/latest/getting-started-with-etcd.html) 
 - [etcd @ github](https://github.com/etcd-io/etcd)
 - [goreman](https://github.com/mattn/goreman)
