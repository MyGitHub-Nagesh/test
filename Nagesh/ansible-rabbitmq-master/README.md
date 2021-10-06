Rabbitmq 
========


Playbook that installs and configures RabbitMQ message broker.

Supports multiple cluster deployment, it is based on
Mayeu.RabbitMQ role, without SSL or federation support.

## Installation

Use Ansible galaxy to install this playbook:

    $ ansible-galaxy install openstack-ansible-galaxy.rabbitmq

## Supported system

Ubuntu 14.04 (Trusty), Ubuntu 16.04 (Xenial) and CentOS 7

## Role Variables

### Environment

Use `rabbitmq_conf_env` so set Environment variables such as NODENAME,
HOSTNAME, RABBITMQ_USE_LONGNAME, NODE_PORT, NODE_IP_ADDRESS, etc.

Example:

```yaml
rabbitmq_conf_env:
  NODENAME: rabbit1

```

### Configuration file

`rabbitmq_tcp_address` - listening address for the tcp interface, such
as `0.0.0.0`.

`rabbitmq_tcp_port` - listening port for the tcp interface, such as `5672`.

`rabbitmq_cluster` - a boolean variable, when set to `True` the role will add
all nodes in a play group to a cluster setup in a configuration file. It
depends on a `ansible_play_hosts` magic variable, found in ansible 2.2
or later.



### Cluster setup

This role supports setting up a simple cluster by adding all the nodes in a
play group that uses the role. It adds the nodes to `cluster_nodes` section
in rabbitmq.conf file. All the nodes are `disc` nodes. The role also sets the
same "Erlang Cookie" to all the nodes belonging to a cluster. In this way
nodes join the cluster automatically during the bootstrap.

For the initial deployment, it is advised to serialize the node deployment in
a way that, at first, a single node is deployed, followed by all the other
nodes in the second run. This would result in a consistent cluster setup.
Playbook example:

```yaml
  - hosts: rabbitmq
    become: True
    serial:
      - 1
      - '100%'
    roles:
      - rabbitmq
```




## License

BSD
