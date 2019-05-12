# Sandbox Cassandra Cluster

Note: These playbooks are a work-in-progress.  I'm sure other projects like this exist in the wild, but I'm using this to learn more about Ansible for managing Cassandra and to assemble a monitoring solution for cassandra that uses open source tools.

This repository contains the ansible playbooks for building a Cassandra environment with the following components:

* A Cassandra cluster
  - [Apache Cassandra 3.11](http://cassandra.apache.org/)
  - [OpenJDK 1.8](https://openjdk.java.net/)
  - [JMX exporter](https://github.com/prometheus/jmx_exporter)
* A Reaper server, for repair and snapshot management
  - [Reaper](http://cassandra-reaper.io/)
  - [OpenJDK 1.8](https://openjdk.java.net/)
  - [PostgreSQL 10](https://www.postgresql.org/)
* A monitoring server to track the health of the cluster
  - [Grafana](https://grafana.com/)
  - [Prometheus](https://prometheus.io/)

# Requirements

This project assumes you have the following:

* A system to run the playbooks from with Ansible 2.7 or later installed and an ssh key pair generated on it

* Three servers to be used as cassandra nodes with the following minimum specs:
  - CentOS 7 (latest)
  - 2 vCPUS
  - 2.5 GB of RAM
  - 10 GB of disk space

* One server to be used for the Reaper server with the following minimum specs:
  - CentOS 7 (latest)
  - 1 vCPUS
  - 1.2 GB of RAM
  - 10 GB of disk space

* One server to be used for the monitoring server with the following minimum specs:
  - CentOS 7 (latest)
  - 2 vCPUS
  - 1 GB of RAM
  - 10 GB of disk space

Every server the ansible workstation will be managing will need the public key installed in the root user's `~/.ssh/authorized_keys` file.

# Instructions

To use these playbooks check out this repository, go into the playbooks directory and update the cassandra_vars.yml file with the following information:

* The name you want for the cluster
* The IP addresses of the cassandra nodes
* The IP address of the Reaper system
* The IP address of the monitoring system
* The credentials you will use for each component that requires JMX access to the Cassandra cluster

Add the following to the `/etc/hosts` file:

    172.16.123.99   cassmon
    172.16.123.100  reaper
    172.16.123.101  cass1
    172.16.123.102  cass2
    172.16.123.103  cass3

Replace the IP addresses as appropriate.

Then use the following commands to run the playbooks:

    ansible-playbook cassandra-cluster.yml -i systems.ini -u root
    ansible-playbook cassandra-reaper.yml -i systems.ini -u root
    ansible-playbook cassandra-monitoring.yml -i systems.ini -u root
