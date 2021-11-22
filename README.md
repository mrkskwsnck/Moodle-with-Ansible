# Moodle-with-Ansible

Collection of Ansible Playbooks and Roles to deploy and maintain Moodle.

## Cheatsheet

> One-liner for daily tasks.

As a precondition, one must set both environment variables `$INVENTORY` and `$VARS`, to address the desired Moodle system.
For this a copy of the `.env-example` file will suite you, …

```bash
INVENTORY=./inventory/hosts.example.ini
EXTRA_VARS=./playbooks/vars/instance.example.yml
VAULT_PASSWORD_FILE=~/.ansible/vault-passwords/example
```

… where one need to set both paths to the inventory and the variables file, respectively.

```bash
# Set environment variables
eval $(cat .env-test)

# Check environment variables
echo $INVENTORY $EXTRA_VARS $VAULT_PASSWORD_FILE
```

Where the basename of you Moodle′s inventory and instance files is subject to ones liking.

### Toggle maintenance mode

Turn maintenance mode on

```bash
ansible-playbook --inventory $INVENTORY playbooks/deploy_moodle.yml --extra-vars @$EXTRA_VARS --vault-password-file $VAULT_PASSWORD_FILE --tag maintenanceon [--check]
```

Turn maintenance mode off

```bash
ansible-playbook --inventory $INVENTORY playbooks/deploy_moodle.yml --extra-vars @$EXTRA_VARS --vault-password-file $VAULT_PASSWORD_FILE --tag maintenanceoff [--check]
```

### Toggle PHP emergency error logging

Assuming the `config.php` file contains a suitable block. See also [PHP error logs](https://docs.moodle.org/dev/PHP_error_logs)

**Example block**

```php
/* Begin of emergency PHP ERROR LOGGING */

...

/* End of emergency PHP ERROR LOGGING */
```

Turn PHP emergency error logging on

```bash
ansible-playbook --inventory $INVENTORY playbooks/deploy_moodle.yml --extra-vars @$EXTRA_VARS --vault-password-file $VAULT_PASSWORD_FILE --tags config,phperrorloggingon --skip-tag phperrorloggingoff [--check]
```

Turn PHP emergency error logging off

```bash
ansible-playbook --inventory $INVENTORY playbooks/deploy_moodle.yml --extra-vars @$EXTRA_VARS --vault-password-file $VAULT_PASSWORD_FILE --tags config,phperrorloggingoff --skip-tag phperrorloggingon [--check]
```

### Kill all Moodle sessions

```bash
ansible-playbook --inventory $INVENTORY playbooks/deploy_moodle.yml --extra-vars @$EXTRA_VARS --vault-password-file $VAULT_PASSWORD_FILE --tag killallsessions [--check]
```

### Deploy Moodle

```bash
ansible-playbook --inventory $INVENTORY playbooks/deploy_moodle.yml --extra-vars @$EXTRA_VARS --vault-password-file $VAULT_PASSWORD_FILE [--skip-tags maintenanceon,killallsessions,phperrorloggingoff] [--extra-var git_force=yes --check]
```

**ATTENTION:** With `--check` also `--extra-var git_force=yes` is needed, so it would not fail during check. However, do not use that extra var without check!

#### Decrypt multiline vault

Password e.g. _TrustNo1_

```bash
echo '`$ANSIBLE_VAULT;1.1;AES256
61633033646562336131346336366338653230646161383765333839623934613236373033666537
3933633437303566623364626365636432356263396565320a376664636537616539303961346330
36613464643938616637323931313632316239666466633962663961383631373433623539633263
3433356665336363630a343537663132646365366535613438613437653231666633656561393930
3930`' | ansible-vault decrypt
```

### Restart keepalived

```bash
ansible-playbook --inventory $INVENTORY playbooks/restart_keepalived.yml [--tag whichisprimary] [--check]
```

### Purge caches

```bash
ansible-playbook --inventory $INVENTORY playbooks/deploy_moodle.yml --extra-vars @$EXTRA_VARS --vault-password-file $VAULT_PASSWORD_FILE --tag purgecaches [--check]
```

### Copy log files from remote servers

#### Nginx

Access log

```bash
ansible --inventory $INVENTORY webservers --module-name fetch --args "src=/var/log/nginx/access.log dest=fetched/"
```

Error log

```bash
ansible --inventory $INVENTORY webservers --module-name fetch --args "src=/var/log/nginx/error.log dest=fetched/"
```

#### PHP FastCGI Process Manager (FPM)

```bash
ansible --inventory $INVENTORY webservers --module-name fetch --args "src=/var/log/php7.3-fpm.log dest=fetched/"
```

#### Redis

```bash
ansible --inventory $INVENTORY rediscluster --module-name fetch --args "src=/etc/redis/redis.conf dest=fetched/"
```

#### Sentinel

```bash
ansible --inventory $INVENTORY rediscluster --module-name fetch --args "src=/etc/redis/sentinel.conf dest=fetched/"
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
