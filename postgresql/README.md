
# PostgreSQL Role

A role to configure PostgreSQL with Master and Replica setup using Ansible. This role supports:
- Installing PostgreSQL
- Configuring Master and Replica
- Setting up custom data directories
- Creating PostgreSQL users and databases
- Managing replication

## Requirements

This role assumes the following:
- Supported PostgreSQL version: `12` (default, configurable)
- Target hosts have Python 3 installed
- ACL support is installed on the target hosts
- Vagrant is used for provisioning test platforms

## Role Variables

The following variables can be defined to customize the behavior of the role:

```yaml
# Default PostgreSQL version
postgresql_version: "12"

# Path to PostgreSQL configuration directory
postgresql_conf_path: "/etc/postgresql/{{ postgresql_version }}/main"

# PostgreSQL replication credentials
replication_user: "replicator"
replication_password: "securepassword"

# PostgreSQL data directory
data_directory: "/var/lib/postgresql/{{ postgresql_version }}/main"

# PostgreSQL users to create
postgresql_users:
  - name: app_user
    password: app_password

# PostgreSQL databases to create
postgresql_databases:
  - name: app_db
    owner: app_user
```

## Role Structure

Below is a brief overview of the role structure:

```
roles/
└── postgresql/
    ├── defaults/
    │   └── main.yml
    ├── files/
    ├── handlers/
    │   └── main.yml
    ├── meta/
    │   └── main.yml
    ├── molecule/
    │   └── default/
    │       ├── converge.yml
    │       ├── create.yml
    │       ├── destroy.yml
    │       ├── molecule.yml
    │       └── verify.yml
    ├── tasks/
    │   ├── main.yml
    │   ├── master.yml
    │   └── replica.yml
    ├── templates/
    │   ├── pg_hba.conf.j2
    │   └── postgresql.conf.j2
    └── vars/
        └── main.yml
```

## Usage

### Playbook Example

```yaml
---
- name: Configure PostgreSQL Master and Replica
  hosts: all
  become: yes
  roles:
    - role: postgresql
```

### Molecule Testing

This role includes a Molecule testing scenario (`molecule/default`). The scenario includes:
- Dependency installation
- Syntax checks
- Creation and preparation of instances
- Convergence testing
- Idempotence testing
- Verification of the PostgreSQL setup

Run the following command to execute the tests:

```bash
molecule test
```

### Supported Platforms

This role has been tested on:
- Ubuntu 20.04 using Vagrant

## Template Configuration

- **pg_hba.conf.j2**: Configures host-based authentication for PostgreSQL.
- **postgresql.conf.j2**: Configures core PostgreSQL settings, including `data_directory` and `listen_addresses`.

## Handlers

- **Restart PostgreSQL**: Ensures that PostgreSQL is restarted after configuration changes.

```yaml
---
- name: Restart PostgreSQL
  ansible.builtin.service:
    name: postgresql
    state: restarted
```

## Example Variables

You can override the default variables in your playbook as follows:

```yaml
---
postgresql_version: "13"
replication_user: "replica_user"
replication_password: "replica_pass"
data_directory: "/custom/data/directory"
postgresql_users:
  - name: custom_user
    password: custom_password
postgresql_databases:
  - name: custom_db
    owner: custom_user
```

## License

MIT

## Author

[MorCherlf](https://github.com/MorCherlf)
