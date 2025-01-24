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
# Configure restic backup (add hosts into `restic_server` or `restic_clients`)
- name: Setup restic backup
  ansible.builtin.import_playbook: dreknix.restic.playbook
  vars:
    restic_backup_proto: "sftp"
    restic_backup_user: "dreknix"
    restic_backup_host: "dreknix.example.org"
    restic_backup_ssh_key_file: "~/.ssh/id_restic"

    restic_backup_cmd: >
      restic
        -o sftp.command='ssh {{ restic_backup_user }}@{{ restic_backup_host}}
            -i {{ restic_ssh_key_file }}
            -s sftp'

    restic_backup_report_email: "dreknix@example.com"
    restic_backup_report_on_success: false
    restic_backup_report_on_failure: true
```

    restic_backup_repository: "{{ restic_backup_proto }}::path/to/repository"
    restic_backup_password:

For more variables see
[`roles/clients/templates/.env.j2`](roles/clients/templates/.env.j2).

## Available Tags and Groups

The tag `restic` activates the playbook of the collection. The binary `restic`
will be installed on all hosts in the groups `restic_server` and
`restic_clients`. The [restic backup script](
https://github.com/dreknix/tools-restic-backup) will only be installed and
configured on hosts of the group `restic_clients`.

## License

[MIT](https://github.com/dreknix/ansible-collection-restic/blob/main/LICENSE)
