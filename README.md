# Ansible Role: Backup for Simple Servers

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-backup.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-backup)

Back up Linux servers with a simple Rsync-and-Cron-based solution.

## Requirements

Requires the following to be installed:

  - rsync
  - cron

MySQL or a MySQL-compatible database needs to be installed if you'd like to enable MySQL database backups.

It's also assumed you have a server running somewhere that can accept backup data via Rsync, and on this backup server, you need to install `rsync`, and configure accounts with SSH authentication that allows this role to deliver backups to a specific directory via SSH.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    backup_cron_job_state: present
    backup_hour: "3"
    backup_minute: "00"

Controls whether the backup script is called via a managed cron job. You should stagger backup times among servers so your backup server doesn't get a huge influx of data at once.

    backup_user: "{{ ansible_env.SUDO_USER | default(ansible_env.USER, true) | default(ansible_user_id, true) }}"

User under which backup jobs will run.

    backup_path: /home/{{ backup_user }}/backups

Path to where backups configuration will be stored. Generally speaking, you should use a special backup user account, but you can set this to whatever account has the proper access to the directories you need to back up.

    backup_directories:
      - /home/{{ backup_user }}/domains
      - /home/{{ backup_user }}/repositories

Directories to back up. `{{ backup_user }}` must have read access to these dirs. Each directory will be synchronized to the backup server via a separate `rsync` command in the backup script.

    backup_exclude_items:
      - .DS_Store
      - cache
      - tmp

Items to exclude from backups. Each item will be added as a new line in an excludes file used by the backup `rsync` command. Read [this article](http://articles.slicehost.com/2007/10/10/rsync-exclude-files-and-folders) for an explanation of how the `--exclude` option works.

    backup_identifier: id_here
    backup_remote_connection: user@backup.example.com

Options to control where the backup is delivered. It's assumed you'll be routing backups to a backup server via SSH. SSH key management and authentication should be managed separately from this role.

    backup_remote_base_path: "~/backups"

The full path on the remote backup server where backups will be stored (all backups for each server are inside a directory named by the `backup_identifier`).

    backup_remote_host_name: ''
    backup_remote_host_key: ''

Add the remote host key details to ensure the host key is present and there are no SSH connection errors based on the key authentication. Leave blank if you've disabled host key checking or if the host key is already added to the server via some other mechanism.

    backup_remote_connection_ssh_options: ''

Add SSH connection options (e.g. `-p [port]`), as documented in the [SSH command manual](http://man.openbsd.org/ssh).

    backup_mysql: true
    backup_mysql_user: dbdump
    backup_mysql_password: password
    backup_mysql_credential_file: ''

Options for backing up MySQL (or MySQL-compatible) databases. Note the `ansible_ssh_user` used when running this role must be able to add MySQL users for this functionality to be managed by this role.
Instead of creating a new MySQL user account you can provide an existing one using `backup_mysql_credential_file` an option file as documented in the [End-User Guidelines for Password Security](https://dev.mysql.com/doc/refman/5.7/en/password-security-user.html).

## Dependencies

None.

## Example Playbook

    - hosts: servers
    
      vars:
        backup_identifier: "{{ inventory_hostname|replace('.', '') }}"
        backup_user: "backupuser"
        backup_remote_connection: user@backups.example.com
        backup_hour: "1"
        backup_minute: "15"
        backup_mysql: false
        backup_directories:
          - /etc/myapp
          - /var/myapp/data
          - /home/myuser
    
      roles:
        - geerlingguy.backup

## License

MIT / BSD

## Author Information

This role was created in 2017 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
