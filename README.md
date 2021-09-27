# Moodle-with-Ansible

Collection of Ansible Playbooks and Roles to deploy and maintain Moodle.

# Cheatsheet

**Toggle maintenance mode**

Turn maintenance mode on

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --tag maintenanceon [--check]  # Maintenance on
```

Turn maintenance mode off

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --tag maintenanceoff [--check]  # Maintenance off
```

**Kill all Moodle sessions**

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --tag killallsessions [--check]
```

**Deploy Moodle**

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --skip-tags [maintenanceon,maintenanceoff] [--extra-var git_force=yes --check]
```

**Purge caches**

```bash
ansible-playbook --inventory inventory/$FARM.ini playbooks/deploy_moodle.yml --extra-vars @playbooks/vars/$INSTANCE.yml --vault-password-file $HOME/.ansible/vault-passwords/moodle_$INSTANCE --tag purgecaches [--check]
```

## License

> Collection of Ansible Playbooks and Roles to deploy and maintain Moodle.
> Copyright © 2021  Markus Kwaśnicki
> 
> This program is free software: you can redistribute it and/or modify
> it under the terms of the GNU General Public License as published by
> the Free Software Foundation, either version 3 of the License, or
> (at your option) any later version.
> 
> This program is distributed in the hope that it will be useful,
> but WITHOUT ANY WARRANTY; without even the implied warranty of
> MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
> GNU General Public License for more details.
> 
> You should have received a copy of the GNU General Public License
> along with this program.  If not, see https://www.gnu.org/licenses/gpl-3.0.html
