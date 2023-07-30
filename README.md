# Personal Infra Documentation

Central documentation for Personal Infra project.

## Repositories

### Docs

Project documentation (this repository)

- Infra Docs: https://github.com/N0zz/infra-docs

### Terraform Projects

Terraform variables for different projects/environments. Terraform cloud uses those repo to run terraform. Utilizes modules listed below.

- Test: https://github.com/N0zz/infra-tf-test
- Prod: https://github.com/N0zz/infra-tf-prod

### Terraform Modules

Terraform modules responsible for creating resources. Used by Terraform Projects listed above.

- OVH: https://github.com/N0zz/infra-tfmod-ovh
- UptimeRobot: https://github.com/N0zz/infra-tfmod-uptimerobot
- CloudFlare: https://github.com/N0zz/infra-tfmod-cloudflare

### Ansible

Ansible ansible.cfg, inventory, roles and playboks, as well as ansible docker runner to keep everything neat.

- Ansible: https://github.com/N0zz/infra-ansible-main

## ENV Variables

Required ENV variables in Terraform Run Environment.

### OVH

https://registry.terraform.io/providers/ovh/ovh/latest/docs#provider-configuration

https://www.ovh.com/auth/api/createToken

Variables:

- OVH_APPLICATION_KEY
- OVH_APPLICATION_SECRET
- OVH_CONSUMER_KEY
- OVH_ENDPOINT

### UptimeRobot

https://registry.terraform.io/providers/vexxhost/uptimerobot/latest/docs#configuration-reference

https://uptimerobot.com/dashboard#mySettings

Variables:

- UPTIMEROBOT_API_KEY

### CloudFlare

https://registry.terraform.io/providers/cloudflare/cloudflare/latest/docs#api_token

https://dash.cloudflare.com/profile/api-tokens

Variables:

- CLOUDFLARE_API_TOKEN

## Additionale features/info

### Terraform fmt, tflint, terraform-docs

All Terraform (tf/tfmod) [repositories](#repositories) have configured `.git/hooks/pre-commit` to run `terraform fmt`, `tflint` and `terraform-docs` to ensure repo quality and update `README.md` automatically. All 3 commands are requires to be available on your system to commit to this repository.

On mac systems it should be sufficient to install them via brew:

```bash
$ brew install terraform terraform-docs tflint
$ brew list | grep 'tflint\|terraform'
terraform
terraform-docs
tflint
```

To install brew reffer to [https://brew.sh/](https://brew.sh/) website.

## Infrastructure setup instructions

### 0. Ensure proper ENV vars are set in Terraform Run Environment

VARs described in [ENV Variables](#env-variables).

### 1. Create servers (VMs/Dedicated servers)

#### Requirements

- Debian / Ubuntu System
- Ansible public ssh key added for default system user
- Open ports 80 and 443 for web_server VMs

### 2. Add external (WAN) IPs of new VMs to terraform config

Inside [Terraform Projects](#terraform-projects) repositories in `variables.tf` update old IP addresses.

### 3. Run terraform for `test` and `prod` environments

To create/update DNS records, uptimerobot monitors and alerts.

### 4. Add servers definition to ansible inventory

Update ansible `inventory/hosts` with new hosts data (names/ips/users) in `Ansible` repository ([repositories](#repositories)).

### 5. Run ansible to configure servers

Playbooks:

- play/setup.yml
- play/base.yml
- play/personalize.yml

### 6. Confirm email from UptimeRobot to enable alerts

After Terraform creates alert contacts, UptimeRobot sends confirmation link.
