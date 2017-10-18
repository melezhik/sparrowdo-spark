# Instal Spark cluster with Docker and Sparrowdo

# Caveat

CentOS is only supported platform for the moment.

# Usage


## Install master

TODO

## Install slaves

    # Create Spark/Slaves network

    docker network create --subnet=172.18.0.0/16 spark-net

    # Run CentOS container 
    $ docker run --privileged --entrypoint  init  \
    -v /sys/fs/cgroup:/sys/fs/cgroup:ro --net spark-net \
    --ip 172.18.0.4 \
    -p 2222:22 -t -d  centos --name worker1


    # Deploy Sparky on running container

    $ sparrowdo \
    --docker=worker1 \
    --no_sudo \
    --sparrowfile=sparrowfile-slave \
    --format=production --bootstrap
