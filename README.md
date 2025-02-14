# Ansible Collection for Setup Restic Backup System

A collection of roles for configuring `restic` based backup. For the backup task
the [restic script](https://github.com/dreknix/tools-restic-backup) is used. The
roles are currently only supporting Debian or Ubuntu based distros.

## Usage

Configure `collections_path` in `ansible.cfg`.

Insert collection into `requirements.yml`:

``` yaml
collections:
  - name: dreknix.restic
    source: https://github.com/dreknix/ansible-collection-restic.git
    type: git
    version: main
```

Use Galaxy to install the collection:

``` console
ansible-galaxy install --force-with-deps -r requirements.yml
```

Import the playbook of the collection and overwrite the default values of the
collection variables:

``` yaml
# Configure restic backup (add hosts into `restic_servers` or `restic_clients`)
- name: Setup restic backup
  ansible.builtin.import_playbook: dreknix.restic.playbook
  vars:
    restic_backup_proto: "sftp"
    restic_backup_user: "dreknix"
    restic_backup_host: "dreknix.example.org"
    restic_backup_repository_path: restic/{{ inventory_hostname }}

    restic_backup_ssh_key_file: "~/.ssh/id_restic"
    restic_backup_ssh_create_config: true

    restic_backup_report_email: "dreknix@example.com"
    restic_backup_report_on_success: false
    restic_backup_report_on_failure: true

    restic_backup_prom_directory: /var/lib/prometheus/node-exporter

    restic_backup_paths:
      - /

    restic_backup_exclude: |
      ...list of files...
```

For more variables see
[`roles/clients/templates/.env.j2`](roles/clients/templates/.env.j2). The
variables `restic_backup_repository` and `restic_back_cmd` are automatically
created (see [`roles/clients/default/all.yml`](roles/clients/default/all.yml)).

The variable `restic_install_type` specifies who restic will be installed on the
system.

* `ansible` - The collection installs a binary directly from GitHub. The version
  of the executable can be controlled by setting `restic_version`.
* `system` - The package manager if used to install restic.
* `other` - The executable will be installed by other options outside this
  collection. A test if the executable is available is performed.

## Available Tags and Groups

The tag `restic` activates the playbook of the collection. The binary `restic`
will be installed on all hosts in the groups `restic_servers` and
`restic_clients`. The [restic backup script](
https://github.com/dreknix/tools-restic-backup) will only be installed and
configured on hosts of the group `restic_clients`.

## License

[MIT](https://github.com/dreknix/ansible-collection-restic/blob/main/LICENSE)
