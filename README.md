# Ansible PostgreSQL role [![Build Status](https://travis-ci.org/cimon-io/ansible-role-postgresql.svg?branch=master)](https://travis-ci.org/cimon-io/ansible-role-postgresql)

This is a fork of [ANXS/PostgreSQL](https://github.com/ANXS/postgresql).

An Ansible role that installs and configures PostgreSQL, extensions, databases and users.

## Installation

The role has been tested on Ansible 2.8 and higher.

To install, run the ansible-galaxy command:

```
ansible-galaxy install cimon-io.postgresql
```

## Dependencies

- role: ANXS.monit ([Galaxy](https://galaxy.ansible.com/ANXS/monit/)/[GH](https://github.com/ANXS/monit)) - an Ansible role which installs monit monitoring and management tool (if you want monit protection, you should set `monit_protection: true`).

## Variables

Available variables are listed below, along with default values (for more options, see `defaults/main.yml`):

```yaml
# Basic settings
postgresql_version: 12
postgresql_encoding: 'UTF-8'
postgresql_locale: 'en_US.UTF-8'
postgresql_ctype: 'en_US.UTF-8'

postgresql_admin_user: "postgres"
postgresql_default_auth_method: "trust"

postgresql_service_enabled: false # should the service be enabled, default is true

postgresql_cluster_name: "main"
postgresql_cluster_reset: false

# List of databases to be created (optional)
# Note: for more flexibility with extensions use the postgresql_database_extensions setting.
postgresql_databases:
  - name: foobar
    owner: baz          # optional; specify the owner of the database
    hstore: yes         # flag to install the hstore extension on this database (yes/no)
    uuid_ossp: yes      # flag to install the uuid-ossp extension on this database (yes/no)
    citext: yes         # flag to install the citext extension on this database (yes/no)
    encoding: 'UTF-8'   # override global {{ postgresql_encoding }} variable per database
    lc_collate: 'en_GB.UTF-8'   # override global {{ postgresql_locale }} variable per database
    lc_ctype: 'en_GB.UTF-8'     # override global {{ postgresql_ctype }} variable per database

# List of database extensions to be created (optional)
postgresql_database_extensions:
  - db: foobar
    extensions:
      - hstore
      - citext

# List of users to be created (optional)
postgresql_users:
  - name: baz
    pass: pass
    encrypted: no       # denotes if the password is already encrypted.

# List of user privileges to be applied (optional)
postgresql_user_privileges:
  - name: baz                   # user name
    db: foobar                  # database
    priv: "ALL"                 # privilege string format: example: INSERT,UPDATE/table:SELECT/anothertable:ALL
    role_attr_flags: "CREATEDB" # role attribute flags
```

## Testing

This project comes with a `Vagrantfile`, this is a fast and easy way to test changes to the role, fire it up with `vagrant up`. See [vagrant docs](https://docs.vagrantup.com/v2/) for getting setup with vagrant.

Once your VM is up, you can reprovision it using either `vagrant provision`, or `ansible-playbook tests/playbook.yml -i vagrant-inventory`.

If you want to toy with the test play, see [tests/playbook.yml](./tests/playbook.yml), and change the variables in [tests/vars.yml](./tests/vars.yml).

If you are contributing, please, first test your changes within the vagrant environment (using the targeted distribution), and if possible, ensure your change is covered with the tests found in [.travis.yml](./.travis.yml).

## License

Licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.

## Feedback

Feedback, bug-reports and requests are [welcome](https://github.com/cimon-io/ansible-role-postgresql/issues)!
