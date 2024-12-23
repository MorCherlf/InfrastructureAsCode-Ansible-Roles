
# Prometheus Role Documentation

## Overview

The `prometheus` role automates the installation and configuration of Prometheus for monitoring systems and services.

## Requirements

- Ansible 2.9 or higher
- Ubuntu or Debian-based distributions
- Sudo privileges

## Role Variables

| Variable         | Description                            | Default Value |
|-------------------|----------------------------------------|---------------|
| `prometheus_port`| The port Prometheus listens on         | `9090`        |
| `prometheus_job_name`| The default prometheus job's name  | `prometheus`  |
| `prometheus_data_path` | Path to the Prometheus data folder | `/var/lib/prometheus` |
| `prometheus_config_path` | Path to the Prometheus configuration file | `/etc/prometheus/prometheus.yml` |


## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  become: yes
  roles:
    - role: prometheus
```

## Molecule Testing

This role includes Molecule scenarios for testing.

### Run Tests

1. Install dependencies:
   ```bash
   pip install molecule[docker]
   ```

2. Run the default scenario:
   ```bash
   molecule test
   ```

## License

MIT

## Author

[MorCherlf](https://github.com/MorCherlf)
