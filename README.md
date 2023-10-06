# Ansible role linux_sensors

Ansible role to install Linux sensor modules for monitoring hardware temperature and SSD (SMART) status.

> Be aware that this role is opinionated and fits my preferences and way of working.
> It may or may not be suitable for your needs.

## Role variables

### sensor_packages

Defines the list of packages needed to monitor the disk and motherboard temperature and status.

### samsung_ssd_db

Boolean to specify whether or not the Samsung EVO 870 and 890 Pro SSD information should be added to the database of `hdd_temp`.

### Mandatory variables

[None.]

### Optional parameters (with defaults)

```yaml
# Linux packages for reading hardware sensors
sensor_packages:
  - smartmontools
  - lm-sensors
```

```yaml
samsung_ssd_db: false
```

## Usage of this role

To use this role directly, include the following section in a `requirements.yml` file in the local `roles` directory:

```yaml
# Include the 'linux_sensors` role from GitHub
- src: git@github.com:crazyelectron-io/ansible-role-linux_sensors.git
  scm: git
  version: main
  name: linux_sensors
```

> Only include the 'top' roles, dependencies - when listed in `meta/main.yaml` of the imported role - will be downloaded automatically.

To retrieve roles like this in your project, run `ansible-galaxy install -r roles/requirements.yaml`.
Because these roles will not be updated locally when the repository is changed, to refresh an already retrieved role use `ansible-galaxy install -f -r roles/requirements.yaml`

## Dependencies

[None.]

## Suggested project structure

```shell
├── inventories
│   ├── dev
│   │   ├── group_vars/
│   │   └── hosts.yaml
│   └── prod
│       ├── group_vars/
│       └── hosts.yaml
├── group_vars/
├── host_vars/
├── files/
├── templates/
├── roles
│   ├── local
│   │   ├── local_role1/
│   │   └── local_role2/
│   ├── requirements.yml
│   ├── .gitignore
├── ansible.cfg
├── README.md
├── some_playbook.yaml
├── other_playbook.yaml
```

Create a `roles/.gitignore` file to exclude the downloaded roles:

```ini
#Ignore everything in roles dir...
/*
# ... but current file...
!.gitignore
# ... external role requirement file
!requirements.yaml
# ... and configured custom/local roles
!local*/
```

Add `roles_path = roles` to `ansible.cfg` to make sure that roles are searched and downloaded in our local folder.

### Workflow to deploy from the project

1. Clone the project repository
2. Download the external roles: `ansible-galaxy install -r roles/requirements.yaml`
3. Launch your playbook: `ansible-playbook -i inventories/dev [some_playbook].yaml -u ANSIBLE_USER`
