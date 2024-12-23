
# Ansible Role: `api_deploy`

This Ansible role deploys and manages the [`pharmacy-app`](https://github.com/MorCherlf/OpenAPI-PharmacyDemo) API application using Docker Compose.

## Requirements

- Docker and Docker Compose must be installed on the target host.
- The `docker_deploy` role should be applied beforehand to set up the Docker environment.

## Role Variables

| Variable              | Description                                                   | Default Value                  |
|-----------------------|---------------------------------------------------------------|--------------------------------|
| `docker_compose_dir`  | Directory where the Docker Compose file will be placed        | `/opt/docker-compose-pharmacy-app` |
| `pharmacy_app_image`  | Docker image to use for the Pharmacy App                     | `morcherlf/pharmacy-demo:latest` |
| `pharmacy_app_port`   | Port to expose the application                                | `8080`                         |
| `pharmacy_app_env`    | Environment variables for the application                    | `['GIN_MODE=release']`         |

## Dependencies

This role has no direct dependencies but works seamlessly with roles that set up Docker environments, such as:

- `docker_deploy`

## Example Playbook

```yaml
- name: Deploy Pharmacy App
  hosts: all
  roles:
    - role: docker_deploy
    - role: api_deploy
      vars:
        pharmacy_app_image: "morcherlf/pharmacy-demo:latest"
        pharmacy_app_port: 8080
        pharmacy_app_env:
          - "GIN_MODE=release"
```

## Molecule Testing

This role is tested using Molecule. The default scenario verifies:

- Docker is running on the target host.
- The `pharmacy-app` container is deployed and running.
- The `pharmacy-app` container is correctly bound to the specified port (`8080`).

### Running Molecule Tests

To test this role, run the following commands:

```bash
molecule test
```

This will perform a complete lifecycle test, including:

- Dependency resolution
- Syntax checking
- Converge (role application)
- Verification of deployment

## License

MIT

## Author

[MorCherlf](https://github.com/MorCherlf)
