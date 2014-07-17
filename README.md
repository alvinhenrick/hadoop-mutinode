# Hadoop (YARN) Multinode Cluster with Docker.

The purpose of this project is to help developer quickly start multinode cluster with docker containers on their laptop.

There are multiple and betters ways to solve the problems and since I am not a DevOps guy please feel free to suggest , advice or contribute to it.

The first docker container **hadoop-base** provisions ubuntu with java and hadoop and does most of the leg work to setup the master and slave containter.The **hadoop-base** is extending from **docker-serf**. See [DNSMASQ/SERF](https://github.com/alvinhenrick/docker-serf)


The second container image **hadoop-dn** extends from hadoop-base and install the slave specific hadoop configuaration and it also installs daemontools to run the sshd , serf and dnsmasq so when we start the docker container in daemon mode it keep running instead of exiting immediatley after startup.


The third docker container **hadoop-nn-dn** extends from hadoop-base and install the master specific hadoop configuaration and it also installs daemontools to run the sshd , serf and dnsmasq.

I decided to start master container in foreground mode therefore when we startup master container it will take you to the bash shell prompt after successfull startup.


The example for demonstration is using 2 node cluster.The master node is also configured as slave node so we have 2 slave nodes and 1 master node.


Prerequisite
------------

1. Docker must be installed on the host computer / laptop.
2. `git clone https://github.com/alvinhenrick/docker-serf`
3. Change directiy to where you cloned the above repository.
4. Run `docker build -t alvinhenrick/serf .`

 
Build Multinode Hadoop Cluster
------------------------------

 
`git clone https://github.com/alvinhenrick/hadoop-mutinode`

* Build hadoop-base container
  * Change directory to hadoop-mutinode/hadoop-base.
  * Run `docker build -t alvinhenrick/hadoop-base .`
  * This will take a while to build the container go grab a cup of coffee or whatever drink you like :)
 
  
* Build hadoop-dn Slave container (DataNode / NodeManager)
  * Change directory to hadoop-mutinode/hadoop-dn.
  * Run `docker build -t alvinhenrick/hadoop-dn .`  

* Build hadoop-nn-dn Master container (NameNode / DataNode / Resource Manager / NodeManager)
  * Change directory to hadoop-mutinode/hadoop-nn-dn.
  * Run `docker build -t alvinhenrick/hadoop-nn-dn .`  

**Sart the containers.**

 * Change directory to hadoop-mutinode.
 * Run `./start-cluster.sh`
 * On BASH shell prompt Run `/usr/local/hadoop/bin/start-hadoop.sh`
 * After startup on prompt type `su - hduser`
 * Run `jps`
 * Run `hdfs dfs -ls /` 
  

     
     
     