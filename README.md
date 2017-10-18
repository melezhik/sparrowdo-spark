# Instal Spark cluster with Docker and Sparrowdo



# Caveat

CentOS is a only supported platform for the moment.

# Usage

## Setup docker

First of all you should create a dedicated network for all Spark instances:

    $ docker network create --subnet=172.18.0.0/16 spark-net

## Install master

Run Docker container for master. You should call docker images master, it is obligatory:

    $ docker run --entrypoint init --net spark-net --ip 172.18.0.2 -t -d centos --name master

Deploy Spark master on running container:

    $ sparrowdo \
    --docker=master \
    --no_sudo \
    --sparrowfile=sparrowfile-master \
    --format=production --bootstrap
  
## Install slaves

Run Docker container for slave:

    $ docker run --entrypoint  init --net spark-net --ip 172.18.0.4 -t -d --name worker1 centos

Deploy Spark slave on running container:

    $ sparrowdo \
    --docker=worker1 \
    --no_sudo \
    --sparrowfile=sparrowfile-slave \
    --format=production --bootstrap


And so on, launch as many slaves as you wish.

## Picking up new slaves

Once you've created a master and some slaves, you need to run [cluster launch script](https://spark.apache.org/docs/latest/spark-standalone.html#cluster-launch-scripts)
so that master found its new slaves.

    $ sparrowdo \
    --docker=master \
    --no_sudo \
    --sparrowfile=sparrowfile-cluster-launch \
    --format=production --bootstrap


# See also

https://spark.apache.org/docs/latest/spark-standalone.html
