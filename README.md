# Vaultwarden Deployment
```
├── ansible-deployment `Ansible Deployment files`
│   ├── default_inventory.yml `Structure for an inventory file`
│   ├── install_ansible.sh `Script to install Python ansibble modules`
│   ├── playbook.yml
│   ├── tasks `Playbook tasks`
│   │   └── templates `Templates for repetitive tasks`
│   ├── templates `Helm values templates`
│   └── vars `General vars file for Playbook`
├── README.md
└── vaultwarden_values `Helm values for Github Actions`
```
## Requirements

- Ansible must be installed. For the required Python modules, script [install_ansible.sh](./ansible-deployment/install_ansible.sh) can be ran (Edit the top variables in the file depending on whether or not a virtual python environment is used)

## Github Action Deployment

- The Github workflow is defined at [.github/workflows/deploy.yml](.github/workflows/deploy.yml)

- The flow checks out the code, then saves the kubeconfig secret as a file

- The Helm Repo for Vaultwarden is then added to the cluster

- The helm chart is then installed in the final step

## Ansible Deployment

- The default inventory is defined at [ansible-deployment/default_inventory.yml](./ansible-deployment/default_inventory.yml)

- Values can be changed depending on setup

- Following command can be ran
    ```sh
    ansible-playbook -i .inventory/inventory.yml ansible-deployment/playbook.yml
    ```

## Assumptions made

- User can portforward to a pod and does not need access over the internet
- Only a few users will use the app at one time
- Creating a user is not yet necessary and using the admin area will be enough (See https://github.com/dani-garcia/vaultwarden/discussions/6883#discussioncomment-15959538)

## Improvements

### Improvements to deployment strategy

- Security context for pods in the helm values
- Strategy for updates of pods should be configured
- Connect with GatewayAPI/Ingress Controller and a TLS Certificate Manager (e.g. Cert Manager)
- Look further into Helm Chart used

### Improvements to Application

- Further research into application configuration and settings
- Set up external database with Postgres or MariaDB as SQLite may not be performant enough
- Backups which stores info ouside the cluster
- SSO Integration if needed
- More secure admin token
