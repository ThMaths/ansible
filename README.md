# Monitoring Stack with Ansible

This repository contains an Ansible playbook that installs the following monitoring tools :

- **Grafana** for visualising data.
- **Prometheus** for collecting metrics.
- **Loki** for storing logs.
- **Promtail** for shipping logs to Loki.

## Prerequisites

- A target machine running Ubuntu or Debian.
- Ansible installed on the control node from which you run the playbook.

## Inventory Configuration

The file `inventories/inventory.yml` provides an example of a local host:

```yaml
local:
  hosts:
    home:
  vars:
    ansible_connection: local
    ansible_user: mathias
    external_ip: 54.170.159.80
```

Adjust `ansible_user` to match the remote user (not needed for `local` connection) and set `external_ip` to the address you use to reach Grafana.

## Running the Playbook

Execute the playbook from the root of this repository:

```bash
ansible-playbook playbook/install_monitoring.yml -i inventories/inventory.yml
```

This installs and starts Grafana, Prometheus, Loki and Promtail as systemd services.

## Accessing Grafana and Viewing Data

After the installation completes, navigate to:

```
http://<external_ip>:3000
```

The default credentials are `admin` / `admin` (you will be prompted to change the password). Within Grafana:

1. Add a **Prometheus** data source pointing to `http://localhost:9090`.
2. Add a **Loki** data source pointing to `http://localhost:3100`.
3. Create dashboards to explore the collected metrics and logs.

Prometheus is available at `http://<external_ip>:9090` and Loki at `http://<external_ip>:3100` if you want to check their status.

## References

- [Grafana Documentation](https://grafana.com/docs/)
- [Prometheus Documentation](https://prometheus.io/docs/introduction/overview/)
- [Loki Documentation](https://grafana.com/docs/loki/latest/)
