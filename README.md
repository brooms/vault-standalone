# HashiCorp Vault Master Instance

Ansible plays to deploy a Docker image of Vault, initialise and seed it with an initial set of secrets.

The intent is for this instance of Vault to be the single source of truth for secrets. This instance may be used to seed other Vault instances.

## Deployment
![Deployment](images/deployment-topology.png)

This project will deploy a Vault master (single instance) onto a host, it is pre-configured to deploy the master to localhost.

**Prerequisites:**

1. Ansible 2.6+
2. Docker 18.09.0+

To provision the Master Vault instance:

```bash
ansible-playbook playbook.yml
```

A note on the **root token** and **unseal keys**. Ansible will write these to a `key_file.json` file under the `keys` directory. It is important to keep these safe and not lose them otherwise Vault will not be able to be unsealed or accessed in the future.

Vault **must be** unsealed to be used.

## Configuration

Vault is configured via Ansible `group_vars` in the `all.yml` file. As part of the provisioning process a Vault `config.json` file will be generated and bound to the Docker container.

An example configuration is show below:

```yml 
vault:
  # Docker container settings
  image:
    name: vault
    tag: 1.0.0
  # Vault host machine settings (exposed port and ip address of the host machine)  
  host:
    ip: 127.0.0.1
    port: 8200
```
