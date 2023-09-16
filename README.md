

# Local Vault

This repository contains a Docker Compose configuration and a Bash script to create a Vault server in a Docker container. Vault is a tool for managing secrets and protecting sensitive data. In this set up I chose not to run Vault in 'dev' mode because of its in-memory storage. The goal of this setup was to create a persistant and local testing environment.

## Prerequisites

Before you can use this repository, ensure you have the following prerequisites installed:

- Docker: [Install Docker](https://docs.docker.com/get-docker/)
- Docker Compose: [Install Docker Compose](https://docs.docker.com/compose/install/)
- `curl`: You can install it using your system's package manager.
- `jq`: You can install it using your system's package manager.

## How Persistant Storage Is Accomplished

`docker-compose.yml`: In this file we mount our local `/data` directory to Vault's `/vault/config` path to retain data across container restarts.

## Vault (bash script) Commands

### `vault`: This is a bash script that uses the following arguments:
- `start`: Starts the docker container
- `init`: Initializes Vault (This only needs to be done once unless you want a fresh start and decide to delete your `/data` directory that is created when using this argument.)
- `unseal`: Unseals Vault, this will need to be ran after starting the container.
- `stop`: Stop your docker container from running.

 Example commands:

```bash
./vault start
```

```bash
./vault unseal
```

## First Time Setup 

1. Verify you are in the root of this repository.
2. Run `./vault start`
3. Navigate to `http://127.0.0.1:8200` in your browser.
4. Run `./vault init`: This will create `init_values.txt` in your directory which will contain your Vault Token and Unseal Key (VAULT_KEYS_BASE64).
- `VAULT_TOKEN`: This will be used to authenticate to Vault.
- `VAULT_KEYS_BASE64`: This key is needed to unseal Vault and we will be adding it to our Vault bash script in the next step.
5. In  the `vault` script replace under the unseal_vault() function, replace `VAULT_KEYS_BASE64_VALUE_HERE` with the value found in `init_values.txt`
- Note: There are other (more secure) methods to go about this.
6. Run `./vault unseal`
7. You should now be able to authenticate to Vault using the `VAULT_TOKEN` value found in `init_values.txt`.
