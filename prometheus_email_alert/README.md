# prometheus_email_alert Ansible Role

## Overview
The `prometheus_email_alert` role configures Prometheus and Alertmanager to enable email alert functionality. It ensures the proper installation, configuration, and validation of Prometheus alert rules and Alertmanager email settings.

## Role Structure
The role follows the standard Ansible role structure with the following directories:

- `defaults/` - Contains default variables for the role.
- `files/` - Contains static files such as `alert.rules.yml`.
- `handlers/` - Defines handlers to restart services after configuration changes.
- `meta/` - Metadata for the role.
- `molecule/default/` - Molecule scenarios for testing the role.
- `tasks/` - Main tasks for installing and configuring Prometheus and Alertmanager.
- `templates/` - Jinja2 templates for dynamic configurations such as `alertmanager.yml.j2`.
- `vars/` - Additional variables (if required).

## Requirements
- Supported OS: Any Linux distribution with systemd and apt package manager.
- Ansible version >= 2.9.
- Prometheus and Alertmanager binaries available in the system package manager.

## Role Variables
Defined in `defaults/main.yml`:

```yaml
alertmanager_config_file: /etc/prometheus/alertmanager.yml
alert_email_to: "example@example.com"
alert_email_from: "prometheus@example.com"
alert_email_smtp_smarthost: "smtp.example.com:587"
alert_email_smtp_auth_username: "user@example.com"
alert_email_smtp_auth_password: "password"
alert_email_smtp_require_tls: true
alertmanager_service_name: "prometheus-alertmanager"
prometheus_service_name: "prometheus"
prometheus_config_file: "/etc/prometheus/prometheus.yml"
prometheus_alert_rules_file: "/etc/prometheus/rules/alert.rules.yml"
alertmanager_http_port: 9093
prometheus_http_port: 9090
```

## Tasks
1. **Ensure Alertmanager is installed:** Installs `prometheus-alertmanager` using the system package manager.
2. **Configure Alertmanager:** Deploys `alertmanager.yml` from a Jinja2 template with SMTP settings for email alerts.
3. **Set up Prometheus alert rules:** Copies the `alert.rules.yml` file and ensures Prometheus is configured to include it.
4. **Validate configurations:** Uses `promtool` to validate Prometheus configuration and alert rules.
5. **Start and enable services:** Ensures Prometheus and Alertmanager services are enabled and running.

## Handlers
- `Restart Alertmanager`
- `Restart Prometheus`

## Example Playbook
```yaml
- name: Configure Prometheus Email Alerts
  hosts: all
  roles:
    - role: prometheus_email_alert
      vars:
        alert_email_to: "alerts@example.com"
        alert_email_from: "monitor@example.com"
        alert_email_smtp_smarthost: "smtp.example.com:587"
        alert_email_smtp_auth_username: "user@example.com"
        alert_email_smtp_auth_password: "securepassword"
```

## Molecule Tests
Molecule is used for role testing. The `molecule/default/verify.yml` file includes tasks to verify the following:

1. Alertmanager service is active.
2. Prometheus configuration includes the alert rules file.
3. Alert rules file exists and is valid.
4. Alertmanager HTTP API is accessible.
5. Email alert configuration is present in Alertmanager.

Run the tests with:
```bash
molecule test
```

## License

MIT

## Author

[MorCherlf](https://github.com/MorCherlf)