# Moodle-with-Ansible

Collection of Ansible Playbooks and Roles to deploy and maintain Moodle.

## Cheatsheet

One-liner for daily tasks.

### Toggle maintenance mode

Turn maintenance mode on

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --tag maintenanceon [--check]  # Maintenance on
```

Turn maintenance mode off

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --tag maintenanceoff [--check]  # Maintenance off
```

### Kill all Moodle sessions

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --tag killallsessions [--check]
```

### Deploy Moodle

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE [--skip-tags maintenanceon,maintenanceoff] [--extra-var git_force=yes --check]
```

**ATTENTION:** With `--check` also `--extra-var git_force=yes` is needed, so it would not fail during check. However, do not use that extra var without check!

### Restart keepalived

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/restart_keepalived.yml [--tag whichisprimary] [--check]
```

### Purge caches

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --tag purgecaches [--check]
```

### Copy log files from remote servers

#### Nginx

Access log

```bash
ansible --inventory inventory/$FARM.ini webservers --module-name fetch --args "src=/var/log/nginx/access.log dest=fetched/$FARM"
```

Error log

```bash
ansible --inventory inventory/$FARM.ini webservers --module-name fetch --args "src=/var/log/nginx/error.log dest=fetched/$FARM"
```

#### PHP FastCGI Process Manager (FPM)

```bash
ansible --inventory inventory/$FARM.ini webservers --module-name fetch --args "src=/var/log/php7.3-fpm.log dest=fetched/$FARM"
```

#### Redis

```bash
ansible --inventory inventory/$FARM.ini rediscluster --module-name fetch --args "src=/etc/redis/redis.conf dest=fetched/$FARM"
```

#### Sentinel

```bash
ansible --inventory inventory/$FARM.ini rediscluster --module-name fetch --args "src=/etc/redis/sentinel.conf dest=fetched/$FARM"
```

## License

```
Collection of Ansible Playbooks and Roles to deploy and maintain Moodle.
Copyright © 2021  Markus Kwaśnicki

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see https://www.gnu.org/licenses/gpl-3.0.html
```
