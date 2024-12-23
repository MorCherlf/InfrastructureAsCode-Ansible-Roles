
# Grafana Ansible Role

This Ansible role is designed to install and configure Grafana, including its dashboards, datasources, and various configurations.

## Features

- Installs Grafana using Aliyun's mirror APT repository.
- Configures Grafana's `grafana.ini` settings using a Jinja2 template.
- Configures datasources and dashboards using provisioning files.
- Deploys predefined Loki and Prometheus dashboards.
- Ensures Grafana service is enabled and running.

## Requirements

- Ansible 2.9 or later.
- Supported operating systems: Debian-based systems with APT package manager.

## Role Variables

The following variables can be customized in the `defaults/main.yml` file:

### Grafana Service Configuration
```yaml
grafana_http_port: 3000  # Grafana server HTTP port
grafana_root_url: http://localhost:3000  # Root URL
grafana_disable_sign_up: true  # Disable sign-up feature
grafana_auth_anonymous: false  # Disable anonymous authentication
grafana_admin_user: admin  # Admin username
grafana_admin_password: admin  # Admin password
```

### Loki Datasource Configuration
```yaml
loki_datasource_url: http://localhost:3100  # Loki datasource URL
```

### Prometheus Datasource Configuration
```yaml
prometheus_datasource_url: http://localhost:9090  # Prometheus datasource URL
```

### Grafana Dashboard Configuration
```yaml
grafana_dashboard_provider_name: Default Dashboard Provider  # Provider name
grafana_dashboard_update_interval: 30  # Update interval in seconds
grafana_dashboard_path: /var/lib/grafana/dashboards  # Path for dashboard JSON files
```

## Templates

### grafana.ini.j2

This template is used to generate the `grafana.ini` configuration file:
```ini
[server]
http_port = {{ grafana_http_port }}
root_url = {{ grafana_root_url }}

[security]
disable_sign_up = {{ grafana_disable_sign_up }}

[auth.anonymous]
enabled = {{ grafana_auth_anonymous }}

[users]
default_admin_username = {{ grafana_admin_user }}
default_admin_password = {{ grafana_admin_password }}
```

### grafana-datasource.yaml.j2

This template configures the datasources for Grafana:
```yaml
apiVersion: 1
datasources:
  - name: Loki
    type: loki
    access: proxy
    url: {{ loki_datasource_url }}
    basicAuth: false
    isDefault: true
    editable: true
  - name: Prometheus
    type: prometheus
    access: proxy
    url: {{ prometheus_datasource_url }}
    basicAuth: false
    isDefault: false
    editable: true
```

### grafana-dashboard.yaml.j2

This template configures the dashboard provisioning:
```yaml
apiVersion: 1
providers:
  - name: "{{ grafana_dashboard_provider_name }}"
    orgId: 1
    folder: "Default Dashboards"
    type: file
    disableDeletion: false
    updateIntervalSeconds: {{ grafana_dashboard_update_interval }}
    options:
      path: {{ grafana_dashboard_path }}
```

## Tasks

### Main Tasks (`tasks/main.yml`)

The main tasks include:
- Adding Aliyun's APT repository for Grafana.
- Installing Grafana.
- Configuring Grafana using `grafana.ini`.
- Deploying datasources and dashboards.
- Ensuring Grafana directories exist.
- Enabling and starting the Grafana service.

### Handlers

The handler ensures the Grafana service is restarted when configurations change.

```yaml
- name: Restart Grafana service
  ansible.builtin.systemd:
    name: grafana-server
    state: restarted
    enabled: yes
  become: yes
```

## Predefined Dashboards

### Loki Overview Dashboard (`files/dashboards/loki-overview.json`)
A pre-configured dashboard for monitoring Loki logs and metrics.

### Prometheus Overview Dashboard (`files/dashboards/prometheus-overview.json`)
A pre-configured dashboard for monitoring Prometheus metrics such as CPU, memory, and storage usage.

## Example Playbook

Below is an example of how to use this role in your playbook:

```yaml
- name: Setup Grafana
  hosts: all
  roles:
    - role: grafana
```

## License

MIT

## Author

[MorCherlf](https://github.com/MorCherlf)

