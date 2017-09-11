## Network Environment
We install Bind9 DNS into the private network to resolve hostname A records and reverse DNS.

## Consul Cluster
We install a Consul cluster for service discovery and DNS

## HA Prometheus Cluster
We install a HA Prometheus cluster for monitoring, metrics, alerting, and consul forwarding

### Overview
* Create a superset DNS environment
* Create [Consul](https://github.com/hashicorp/consul) cluster used for service discovery
* Create [Prometheus](https://github.com/prometheus) Server cluster
* Create [Prometheus AlertManager](https://github.com/prometheus/alertmanager) service on defined nodes
* Create [Prometheus NodeExporter](https://github.com/prometheus/node_exporter) service on defined nodes
* Create Prometheus [Rules](https://prometheus.io/docs/querying/rules/)/[Alerts](https://prometheus.io/docs/alerting/rules/)
* Create [Third Party Exporter](https://github.com/prometheus/consul_exporter)
* Create [Grafana](https://github.com/grafana/grafana) service

### Vagrantfile

To create the machines run:
```
vagrant up
```
This will, by default, create:
* core1.lan - bind9 DNS server, Consul Client
* prometheus1.lan - Prometheus server, Prometheus AlertManager, Grafana server
* prometheus2.lan - Prometheus server, Prometheus AlertManager, Grafana server
* consul1.lan - Consul Server, Consul Client, Prometheus node exporter
* consul2.lan - Consul Server, Consul Client, Prometheus node exporter
* consul3.lan - Consul Server, Consul Client, Prometheus node exporter
* client1.lan - Consul Client, Prometheus node exporter

### Ansible playbooks
The ansible playbooks provision the nodes listed above.
```
cd ansible
ansible-playbook provision_bind9_servers.yaml -i inventory.py -u vagrant -k -b
ansible-playbook update_resolv.yaml inventory.py -i inventory.py -u vagrant -k -b
ansible-playbook provision_prometheus_servers.yaml -i inventory.py -u vagrant -k -b
ansible-playbook provision_prometheus_alertmanager_servers.yaml -i inventory.py -u vagrant -k -b
ansible-playbook provision_prometheus_node_exporter_servers.yaml -i inventory.py -u vagrant -k -b
ansible-playbook provision_prometheus_consul_exporter_servers.yaml -i inventory.py -u vagrant -k -b
ansible-playbook provision_consul_servers.yaml inventory.py -i inventory.py -u vagrant -k -b
ansible-playbook provision_consul_client_servers.yaml inventory.py -i inventory.py -u vagrant -k -b
```

### Notable UIs

**Prometheus Server UIs:**
* prometheus1.lan:9090
* prometheus2.lan:9090

**Grafana UIs:**
* prometheus1.lan:3000
* prometheus2.lan:3000

**Consul UIs:**
* consul1.lan:8500
* consul2.lan:8500
* consul3.lan:8500
