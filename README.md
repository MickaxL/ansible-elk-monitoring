# Ansible ELK Monitoring

This repository provides a complete Ansible automation to deploy and configure an ELK (Elasticsearch, Logstash, Kibana) monitoring stack on a main Ubuntu server, as well as lightweight agents (Filebeat and Metricbeat) on client machines (Ubuntu-based).

## Project Architecture

```
ansible-elk-monitoring/
├── deploy_monitoring.yml           # Main playbook to launch the deployment
├── roles/
│   ├── elk_server/                 # Role for deploying and configuring the ELK stack
│   │   ├── tasks/
│   │   │   ├── main.yml
│   │   │   └── Ubuntu.yml          # OS-specific tasks (Ubuntu)
│   │   ├── templates/              # Jinja2 configuration templates
│   │   │   ├── elasticsearch.yml.j2
│   │   │   └── kibana.yml.j2
│   │   └── vars/
│   │       └── Ubuntu.yml          # Variables for ELK server deployment
│   └── beats_agents/              # Role for deploying Filebeat and Metricbeat
│       ├── tasks/
│       │   ├── main.yml
│       │   └── Ubuntu.yml          # Tasks for Ubuntu clients
│       ├── templates/              # Templates for Beat config files
│       │   ├── filebeat.yml.j2
│       │   └── metricbeat.yml.j2
│       └── vars/
│           └── Ubuntu.yml          # Variables for Beat agents
└── update-system/                 # Optional update role (APT, etc.)
```

## Features

- Automated deployment of:
  - Elasticsearch
  - Kibana
  - Filebeat
  - Metricbeat
- Modular roles for server and agents
- Support for multiple clients (Ubuntu-based)
- Filebeat system module enabled
- Metricbeat system module enabled
- Dashboards and ingest pipelines automatically loaded

## Prerequisites

- Ansible >= 2.10
- Inventory file (`inventory.ini`) with at least one host in groups:
  - `elk_server`
  - `elk_clients`
- Python >= 3.6 on all managed nodes
- SSH access with `become` rights to all servers

## Usage

1. **Clone the repository**:

```bash
git clone https://github.com/your-username/ansible-elk-monitoring.git
cd ansible-elk-monitoring
```

2. **Edit your inventory**:

```ini
[elk_server]
192.168.167.16 ansible_user=mickael

[elk_clients]
192.168.167.31 ansible_user=mickael
192.168.167.32 ansible_user=mickael
```

3. **Run the playbook**:

```bash
ansible-playbook -i inventory.ini deploy_monitoring.yml
```

## Dashboard Access

After deployment, access Kibana at:

```
http://192.168.167.16:5601
```

You should see:
- System logs in the [Filebeat System] dashboards
- System metrics in the [Metricbeat System] dashboards

## Troubleshooting

- **Filebeat/Metricbeat connection error**:
  Make sure the ELK server is reachable (check firewall/Elasticsearch availability).

- **Dashboards not appearing**:
  Ensure `filebeat setup --dashboards` and `metricbeat setup --dashboards` were executed and Kibana is reachable.

- **APT lock errors**:
  Wait for any running `unattended-upgrades` or `dpkg` processes to finish before running the playbook.

## Author

Mickael Paquet

## License

This project is licensed under the MIT License - see the LICENSE file for details.

