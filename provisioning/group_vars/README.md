# Credentials (`credentials.yml`)
## How to encrypt this file
Make sure to be in the directory where the `vagrantfile` is and start the provisioning vm `bpro-provisioning-vm`:
```bash
$ vagrant up bpro-provisioning-vm
```

Any occuring errors can be ignored. Next connect with the vm:
```bash
$ vagrant ssh bpro-provisioning-vm
```

Inside of the vm, go to `/vagrant` and create the file `provisioning/group_vars/credentials.yml` via `ansible-vault`:
```bash
(vm) $ cd "/vagrant"
(vm) $ ansible-vault create "provisioning/group_vars/credentials.yml"
```
```
New Vault password: some_strong_password
Confirm New Vault password: some_strong_password
```

Save the password to the file `.vault_pass`:
```bash
(vm) $ vi ".vault_pass"
```

This is necessary since Ansible is executed within a vm, so it is not possible to have a interactive prompt. Using `ask_vault_pass` in the `vagrantfile` will not work!

## Variables
Insert the following variables:
```yml
---
# generic
VAULT_SERVER_STATIC_IP_ADDRESS: ""
VAULT_SERVER_NET_IP_ADDRESS: ""
VAULT_SDA1_UUID: ""
VAULT_SDA2_UUID: ""

# unattended-upgrades
## postfix credentials
VAULT_RECIPIENT_EMAIL_ADDRESS: ""
VAULT_POSTFIX_HOSTNAME: ""
VAULT_POSTFIX_SMTP_FQDN: ""
VAULT_POSTFIX_SMTP_PORT: ""
VAULT_POSTFIX_RELAY_EMAIL_ADDRESS: ""
VAULT_POSTFIX_RELAY_EMAIL_PASSWORD: ""

# dedyn.io credentials
VAULT_DEDYN_USERNAME: ""
VAULT_DEDYN_PASSWORD: ""
## ddclient settings
VAULT_DDCLIENT_PROTOCOL: ""
VAULT_DDCLIENT_SYNCHRONISE_METHOD: ""
VAULT_DDCLIENT_TARGET: ""
VAULT_DDCLIENT_UPDATE_SERVER: ""
VAULT_DDCLIENT_DYN_DNS_FQDN: ""
## certbot settings
VAULT_LETS_ENCRYPT_EMAIL_ADDRESS: ""
```

## Example
```yml
---
# generic
VAULT_SERVER_STATIC_IP_ADDRESS: "192.168.2.5"
VAULT_SERVER_NET_IP_ADDRESS: "192.168.2.0/24"
VAULT_SDA1_UUID: "845903da-a3b5-4673-be43-f368e35b8dba"
VAULT_SDA2_UUID: "004b127c-e94f-472d-bbd3-fc0c03357b75"

# unattended-upgrades
## postfix credentials
VAULT_RECIPIENT_EMAIL_ADDRESS: "own_username@provider.tld"
VAULT_POSTFIX_HOSTNAME: "bananapi.local"
VAULT_POSTFIX_SMTP_FQDN: "smtp.gmail.com"
VAULT_POSTFIX_SMTP_PORT: "587"
VAULT_POSTFIX_RELAY_EMAIL_ADDRESS: "username@gmail.com"
VAULT_POSTFIX_RELAY_EMAIL_PASSWORD: "password"

# dedyn.io credentials
VAULT_DEDYN_USERNAME: "username.dedyn.io"
VAULT_DEDYN_PASSWORD: "password"
## ddclient settings
VAULT_DDCLIENT_PROTOCOL: "dyndns2"
VAULT_DDCLIENT_SYNCHRONISE_METHOD: "web"
VAULT_DDCLIENT_TARGET: "https://v4.ident.me/"
VAULT_DDCLIENT_UPDATE_SERVER: "update.dedyn.io"
VAULT_DDCLIENT_DYN_DNS_FQDN: "username.dedyn.io"
## certbot settings
VAULT_LETS_ENCRYPT_EMAIL_ADDRESS: "own_username@provider.tld"
```

Save and close the file. For future modifications one can use:
```bash
(vm) $ cd "/vagrant"
(vm) $ ansible-vault edit "provisioning/group_vars/credentials.yml" --vault-password-file=.vault_pass
```

To just view the file:
```bash
(vm) $ cd "/vagrant"
(vm) $ ansible-vault view "provisioning/group_vars/credentials.yml" --vault-password-file=.vault_pass
```

# See also
https://docs.ansible.com/ansible/2.4/vault.html
