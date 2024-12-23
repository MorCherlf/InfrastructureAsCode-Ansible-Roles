
# Docker Deploy Role

This Ansible role is designed to automate the deployment and verification of Docker and Docker Compose on Ubuntu systems. It supports the setup of Docker, Docker Compose, and testing their installation and functionality.

## Role Variables

The role provides the following customizable variables, which are defined in `defaults/main.yml`:

| Variable                  | Default Value                                         | Description                                     |
|---------------------------|-----------------------------------------------------|-------------------------------------------------|
| `docker_compose_version`  | `v2.30.3`                                           | Version of Docker Compose to be installed      |
| `django_app_repo_url`     | `https://github.com/mdn/django-locallibrary-tutorial.git` | Git repository URL for the Django app         |
| `django_app_path`         | `/opt/django-locallibrary-tutorial`                | Path to clone the Django app repository        |
| `django_docker_image`     | `docker.io/timurbabs/django`                       | Docker image for Django                        |
| `django_app_port`         | `8000`                                             | Port for Django application                    |
| `django_app_git_update`   | `true`                                             | Whether to update the Django app repository    |

## Handlers

The role defines a handler to restart the Docker service:

```yaml
- name: Restart Docker service
  ansible.builtin.service:
    name: "{{ docker_service_name }}"
    state: restarted
```

## Tasks

The primary tasks are located in `tasks/main.yml` and perform the following actions:

1. Update the APT package index.
2. Install required Docker packages.
3. Add Docker's GPG key and repository.
4. Install Docker CE.
5. Ensure Docker is running and enabled.
6. Install Docker Compose from a specified version.
7. Verify Docker and Docker Compose installations.
8. Add the `vagrant` user to the Docker group and ensure it has the necessary permissions.

## Molecule Testing

Molecule is used to test the role. The testing scenario is defined in `molecule/default/molecule.yml`. It includes:

- Creating Vagrant instances (`srv1` and `srv2`).
- Preparing the environment by installing Python.
- Converging the role to ensure Docker is properly installed.
- Verifying the role functionality:
  - Docker and Docker Compose installation.
  - Docker container functionality (`hello-world` test).

### Verify.yml

The verification tasks are defined in `molecule/default/verify.yml` and include:

- Checking if the Docker service is active.
- Asserting the installed Docker Compose version.
- Running a Docker container to verify functionality.

## Example Playbook

Below is an example playbook using the role:

```yaml
- hosts: all
  roles:
    - role: docker_deploy
```

## License

MIT

## Author

[MorCherlf](https://github.com/MorCherlf)
